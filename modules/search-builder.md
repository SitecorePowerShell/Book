# SearchBuilder

{% hint style="info" %}
Introduced in SPE 9.0.
{% endhint %}

The SearchBuilder extension library provides a fluent, pipeline-based API for constructing search queries with [`Find-Item`](../appendix/indexing/find-item.md). Instead of building verbose hashtable-based `SearchCriteria` arrays, you chain intuitive filter commands that are easier to discover and less error-prone.

## Getting Started

Load the SearchBuilder library with `Import-Function`:

```powershell
Import-Function -Name SearchBuilder
```

A typical workflow is: create a builder, add filters, then invoke the search.

```powershell
Import-Function -Name SearchBuilder

$search = New-SearchBuilder -Index "sitecore_master_index" -First 25 -LatestVersion
$search | Add-TemplateFilter -Name "Article"
$search | Add-FieldContains -Field "Title" -Value "Welcome"
$results = $search | Invoke-Search

$results.Items | Initialize-Item | ForEach-Object { $_.Name }
```

## Core Commands

| Command | Description |
| :--- | :--- |
| `New-SearchBuilder` | Creates a new builder instance with index, pagination, and query options. |
| `Invoke-Search` | Executes the builder via `Find-Item` and returns a result object. Auto-advances pagination on each call. |
| `Reset-SearchBuilder` | Resets pagination (Skip and PageNumber) to allow re-running from the beginning. |

### New-SearchBuilder Parameters

| Parameter | Description |
| :--- | :--- |
| `-Index` | The search index name. Supports tab completion via `Get-SearchIndex`. |
| `-Path` | Root path to scope the search. |
| `-First` | Page size (number of items per page). |
| `-Skip` | Number of items to skip initially. |
| `-Last` | Return the last N items. |
| `-MaxResults` | Safety cap on total items returned across all pages. |
| `-OrderBy` | Field to sort results by. |
| `-Strict` | Validates field names against the index schema before executing. |
| `-LatestVersion` | Filters to only the latest version of each item. |
| `-IncludeMetadata` | Adds `IndexLastUpdated` to the result object. |
| `-Property` | Select specific `SearchResultItem` properties (e.g., `Name`, `Path`, `TemplateName`) to avoid deserializing full objects. |
| `-QueryType` | Pass a custom `SearchResultItem` subclass for strongly-typed index fields. |
| `-FacetOn` | Fields to facet on, returning category aggregations instead of items. |
| `-FacetMinCount` | Minimum count for a facet value to be included. |

## Filter Commands

Filters are added to the builder through the pipeline. All filter commands mutate the builder in-place.

| Command | Description |
| :--- | :--- |
| `Add-SearchFilter` | Core filter with `-Field`, `-Filter`, `-Value`, `-Invert`, `-Boost`, and `-CaseSensitive` parameters. The `-Filter` parameter has a `ValidateSet` to catch typos at invocation time. |
| `Add-TemplateFilter` | Convenience filter by template `-Name` or `-Id`. |
| `Add-FieldContains` | Shorthand for `Add-SearchFilter` with the `Contains` filter type. |
| `Add-FieldEquals` | Shorthand for `Add-SearchFilter` with the `Equals` filter type. |
| `Add-DateRangeFilter` | Date range filter with relative syntax (`-Last "7d"`, `"2w"`, `"3m"`, `"1y"`) or absolute (`-From`/`-To`). |

## Predicate Grouping

Combine filters with OR/AND logic using filter groups. Groups are CLM-safe and do not use scriptblocks.

```powershell
$search = New-SearchBuilder -Index "sitecore_master_index" -Strict
$group = New-SearchFilterGroup -Operation Or
$group | Add-TemplateFilter -Name "Article"
$group | Add-TemplateFilter -Name "Blog Post"
$search | Add-SearchFilterGroup -Group $group
$search | Add-DateRangeFilter -Field "__Updated" -Last "30d"
$results = $search | Invoke-Search
```

| Command | Description |
| :--- | :--- |
| `New-SearchFilterGroup` | Creates a new filter group with `-Operation` (`Or` or `And`). |
| `Add-SearchFilterGroup` | Adds a completed filter group to the builder. |

## Discovery and Validation

| Command | Description |
| :--- | :--- |
| `Get-SearchFilter` | Lists all 14 valid filter type values with descriptions. |
| `Get-SearchIndexField` | Lists all indexed fields for a given index via `Schema.AllFieldNames`. |

```powershell
Get-SearchFilter                                     # list all filter types
Get-SearchIndexField -Index "sitecore_master_index"   # list all indexed fields
```

Use `-Strict` mode on `New-SearchBuilder` to validate field names against the index schema before executing. This uses `FieldNameTranslator` to resolve logical names (e.g., `__Updated`, `Title`) before checking.

```powershell
$search = New-SearchBuilder -Index "sitecore_master_index" -Strict
$search | Add-SearchFilter -Field "bogus_field" -Filter "Equals" -Value "test"
$search | Invoke-Search
# Throws: "Strict mode: The following fields are not indexed: 'bogus_field'"
```

## Result Object

### Standard Search

| Property | Description |
| :--- | :--- |
| `Items` | The search result items. |
| `HasMore` | `$true` if more pages are available. |
| `PageNumber` | Current page number. |
| `PageSize` | Number of items per page. |
| `TotalCount` | Total matching items in the index. |
| `Truncated` | `$true` if `MaxResults` cap was reached. |
| `MaxResults` | The configured safety cap. |
| `IndexName` | The index that was queried. |
| `Query` | Human-readable query summary. |

With `-IncludeMetadata`, the result also includes `IndexLastUpdated`.

The `Query` property provides full context:

```
_templatename Equals 'Template Folder' AND _fullpath Contains 'powershell' [Path: /sitecore/content | LatestVersion | OrderBy: score]
```

### Faceted Search

When using `-FacetOn`, the result object contains:

| Property | Description |
| :--- | :--- |
| `Facets` | Raw `FacetResults` from the index. |
| `Categories` | Convenience accessor for facet categories. |
| `IndexName` | The index that was queried. |
| `Query` | Human-readable query summary. |

## Examples

### Pagination

Auto-advancing pagination with a `HasMore` signal and safety cap:

```powershell
$search = New-SearchBuilder -Index "sitecore_master_index" -First 100 -MaxResults 500
$search | Add-TemplateFilter -Name "Article"
$all = [System.Collections.ArrayList]@()
do {
    $results = $search | Invoke-Search
    $all.AddRange($results.Items)
} while ($results.HasMore)
Write-Host "Collected $($all.Count) items, truncated: $($results.Truncated)"
```

### Property Projection

Select specific properties for better performance:

```powershell
$search = New-SearchBuilder -Index "sitecore_master_index" -First 10 -Property @("Name", "Path", "TemplateName")
$search | Add-TemplateFilter -Name "Template Folder"
$results = $search | Invoke-Search
$results.Items | ForEach-Object { "$($_.Name) [$($_.TemplateName)]" }
```

{% hint style="info" %}
`-Property` and `-FacetOn` use C# property names (`Name`, `Path`, `TemplateName`), not index field names (`_name`, `_fullpath`, `_templatename`). This matches native `Find-Item` behavior.
{% endhint %}

### Faceted Search

```powershell
$search = New-SearchBuilder -Index "sitecore_master_index" -FacetOn @("TemplateName") -FacetMinCount 50
$search | Add-FieldContains -Field "_fullpath" -Value "powershell"
$results = $search | Invoke-Search
$results.Categories | ForEach-Object {
    Write-Host "$($_.Name):"
    $_.Values | ForEach-Object { Write-Host "  $($_.Name): $($_.AggregateCount)" }
}
```

### Inverted Filter with Boost

```powershell
$search = New-SearchBuilder -Index "sitecore_master_index" -First 5
$search | Add-TemplateFilter -Name "Template Folder"
$search | Add-SearchFilter -Field "_name" -Filter "Contains" -Value "system" -Invert -Boost 5
$results = $search | Invoke-Search
```

### Reset and Re-run

```powershell
$search | Reset-SearchBuilder   # resets Skip and PageNumber to 0
$results = $search | Invoke-Search   # starts from page 1 again
```

## Related Topics

- [Find-Item](../appendix/indexing/find-item.md) — The underlying search command that SearchBuilder wraps
- [Indexing](../appendix/indexing/README.md) — Index management commands
