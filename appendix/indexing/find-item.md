# Find-Item

Finds items using the Sitecore Content Search API.

## Syntax

```text
Find-Item [-Index] <String> [-Criteria <SearchCriteria[]>] [-OrderBy <String>] [-First <Int32>] [-Last <Int32>] [-Skip <Int32>] [<CommonParameters>]
Find-Item [-Index] <String> [-Where <String>] [-WhereValues <Object[]>] [-OrderBy <String>] [-First <Int32>] [-Last <Int32>] [-Skip <Int32>] [<CommonParameters>]
Find-Item [-Index] <String> [-Predicate <Expression<Func<SearchResultItem, bool>>>] [-OrderBy <String>] [-First <Int32>] [-Last <Int32>] [-Skip <Int32>] [<CommonParameters>]
Find-Item [-Index] <String> [-ScopeQuery <String>] [-OrderBy <String>] [-First <Int32>] [-Last <Int32>] [-Skip <Int32>] [<CommonParameters>]
```

## Detailed Description

The Find-Item command searches for items using the Sitecore Content Search API.

© 2010-2019 Adam Najmanowicz, Michael West. All rights reserved. Sitecore PowerShell Extensions

## Parameters

### -Index  &lt;String&gt;

Name of the Index that will be used for the search:

Find-Item -Index sitecore\_master\_index -First 10

| Aliases |  |
| :--- | :--- |
| Required? | true |
| Position? | 1 |
| Default Value |  |
| Accept Pipeline Input? | false |
| Accept Wildcard Characters? | false |

### -Criteria  &lt;SearchCriteria\[\]&gt;

Simple form of search in which logical "AND" operations are needed.

```text
@{ Filter = "Equals"; Field = "_templatename"; Value = "Sample Item"; }, 
@{ Filter = "DescendantOf"; Value = (Get-Item "master:/content/") }
```

Where "Filter" is one of the following values:

* Equals
* StartsWith
* Contains
* ContainsAny
* ContainsAll
* EndsWith
* DescendantOf - performs a _Contains_ with the field **\_path**
* Fuzzy
* InclusiveRange - performs a _Between_ using `int`, `double`, `datetime`, and `string`types
* ExclusiveRange - same as _InclusiveRange_
* MatchesRegex - use something like `^.*$`
* MatchesWildcard - use something like `H?li*m`

Where "Field" is the Index Field name found on the `SearchResultItem` such as the following:

* \_\_smallcreateddate - CreatedDate
* \_group - ID
* \_template - TemplateId
* \_templatename - TemplateName
* \_fullpath - Path

Where "Value" is one of the following:

* string
* string\[\]
* Sitecore.Data.ID
* Sitecore.Data.Items.Item
* Sitecore.Data.Items.Item\[\]
* object\[\] - when using `@()` you'll get this type, which will be treated as an array of strings
* PSObject\[\]
* System.Collections.ArrayList
* System.Collections.Generics.List&lt;string&gt;

Where "Boost" is a positive number greater than 0

Fields by which you can filter can be discovered using the following script:

```text
$criteria = @(
    @{Filter = "StartsWith"; Field = "_fullpath"; Value = "/sitecore/content/" }
)
$props = @{
    Index = "sitecore_master_index"
    Criteria = $criteria
}

Find-Item @props -First 1 | Select-Object -Expand "Fields"
```

| Aliases |  |
| :--- | :--- |
| Required? | false |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | false |
| Accept Wildcard Characters? | false |

### -Where  &lt;String&gt;

```text
$props = @{
    Index = "sitecore_master_index"
    Where = 'TemplateName = @0 And Language=@1'
    WhereValues = "Template Field", "en"
}

Find-Item @props
```

Filtering Criteria using Dynamic Linq syntax: [https://weblogs.asp.net/scottgu/dynamic-linq-part-1-using-the-linq-dynamic-query-library](https://weblogs.asp.net/scottgu/dynamic-linq-part-1-using-the-linq-dynamic-query-library)

| Aliases |  |
| :--- | :--- |
| Required? | false |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | false |
| Accept Wildcard Characters? | false |

### -WhereValues  &lt;Object\[\]&gt;

An Array of objects for Dynamic Linq "-Where" parameter as explained in: [https://weblogs.asp.net/scottgu/dynamic-linq-part-1-using-the-linq-dynamic-query-library](https://weblogs.asp.net/scottgu/dynamic-linq-part-1-using-the-linq-dynamic-query-library)

| Aliases |  |
| :--- | :--- |
| Required? | false |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | false |
| Accept Wildcard Characters? | false |

### -Predicate &lt;Expression&lt;Func&lt;SearchResultItem,bool&gt;&gt;&gt;

Use the `New-SearchPredicate` command to build the appropriate predicates.

| Aliases |  |
| :--- | :--- |
| Required? | false |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | false |
| Accept Wildcard Characters? | false |

### -ScopeQuery &lt;String&gt;

When combined with the Query Builder field, a simple query can be crafted to return search results.

| Aliases |  |
| :--- | :--- |
| Required? | false |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | false |
| Accept Wildcard Characters? | false |

### -OrderBy  &lt;String&gt;

Field by which the search results sorting should be performed. Dynamic Linq ordering syntax used. [https://weblogs.asp.net/scottgu/dynamic-linq-part-1-using-the-linq-dynamic-query-library](https://weblogs.asp.net/scottgu/dynamic-linq-part-1-using-the-linq-dynamic-query-library)

| Aliases |  |
| :--- | :--- |
| Required? | false |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | false |
| Accept Wildcard Characters? | false |

### -First  &lt;Int32&gt;

Number of returned search results.

| Aliases |  |
| :--- | :--- |
| Required? | false |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | false |
| Accept Wildcard Characters? | false |

### -Last  &lt;Int32&gt;

| Aliases |  |
| :--- | :--- |
| Required? | false |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | false |
| Accept Wildcard Characters? | false |

### -Skip  &lt;Int32&gt;

Number of search results to be skipped skip before returning the results commences.

| Aliases |  |
| :--- | :--- |
| Required? | false |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | false |
| Accept Wildcard Characters? | false |

## Outputs

The output type is the type of the objects that the cmdlet emits.

* Sitecore.ContentSearch.SearchTypes.SearchResultItem 

## Notes

Help Author: Adam Najmanowicz, Michael West

## Examples

### EXAMPLE 1

Fields by which filtering can be performed using the -Criteria parameter.

```text
$criteria = @(
    @{Filter = "Equals"; Field = "_templatename"; Value = "Sample Item"}, 
    @{Filter = "Contains"; Field = "Title"; Value = "Sitecore"}
)
$props = @{
    Index = "sitecore_master_index"
    Criteria = $criteria
}

Find-Item @props
```

### EXAMPLE 2

Find items using a search built by the Query Builder field.

```text
$props = @{
    Index = "sitecore_master_index"
    ScopeQuery = "location:{110D559F-DEA5-42EA-9C1C-8A5DF7E70EF9};custom:title|Sitecore"
}

Find-Item @props
```

### EXAMPLE 3

Find all Template Fields using Dynamic LINQ syntax.

```text
$props = @{
    Index = "sitecore_master_index"
    Where = 'TemplateName = @0 And Language=@1'
    WhereValues = "Template Field", "en"
}

Find-Item @props
```

### EXAMPLE 4

Find items using a complex search predicate.

```text
$criteriaTemplate = @{Filter = "Equals"; Field = "_templatename"; Value = "Template Field"; }, @{Filter = "Equals"; Field = "_templatename"; Value = "Sample Item"; Boost=25; }, @{Filter = "Equals"; Field = "_templatename"; Value = "Sample Content"; }
$predicateTemplate = New-SearchPredicate -Operation Or -Criteria $criteriaTemplate

$criteriaContent = @{Filter = "Contains"; Field = "Title"; Value = 'Sitecore'}
$predicateTitle = New-SearchPredicate -Criteria $criteriaContent

$predicateTemplateAndTitle = New-SearchPredicate -First $predicateTemplate -Second $predicateTitle -Operation And

$root = Get-Item -Path "master:" -ID "{110D559F-DEA5-42EA-9C1C-8A5DF7E70EF9}"
$criteriaRoot = @{Filter = "DescendantOf"; Value = $root }
$predicateRoot = New-SearchPredicate -Criteria $criteriaRoot

$predicate = New-SearchPredicate -First $predicateRoot -Second $predicateTemplateAndTitle -Operation And

$props = @{
    Index = "sitecore_master_index"
    Predicate = $predicate
}

Find-Item @props
```

### EXAMPLE 5

Find items using logical AND conditions with ContainsAny. Demonstrates that different array types are handled.

```text
$criteria = @(
    @{Filter = "Equals"; Field = "_templatename"; Value = "Sample Content"; Boost=25; },
    @{Filter = "ContainsAny"; Field = "Title"; Value = [string[]]@('$name','$date')},
    @{Filter = "ContainsAny"; Field = "Title"; Value = @('$name','$date')},
    @{Filter = "ContainsAny"; Field = "Title"; Value = [System.Collections.ArrayList]@('$name','$date')},
    @{Filter = "ContainsAny"; Field = "Title"; Value = [System.Collections.Generic.List[string]]@('$name','$date')}
)
$props = @{
    Index = "sitecore_master_index"
    Criteria = $criteria
}

Find-Item @props
```

### EXAMPLE 6

Find items using logical AND with ContainsAll. Demonstrates looking in multilist fields.

```text
$criteria = @(
    @{Filter = "Equals"; Field = "_templatename"; Value = "Sample Content"; Boost=25; },
    @{Filter = "ContainsAll"; Field = "RelatedImages"; Value = @('{4D427A1D-312D-4EEE-A519-1F5700675BAF}','{4B603402-62AB-4ECB-9CAE-98790DDBC35A}')}
)
$props = @{
    Index = "sitecore_master_index"
    Criteria = $criteria
}

Find-Item @props
```

### EXAMPLE 7

Find an item by ID.

```text
$criteria = @(
    @{Filter = "Equals"; Field = "_group"; Value = "{C89D37FF-3919-4D69-9925-943B67BD22D6}"}
)
$props = @{
    Index = "sitecore_master_index"
    Criteria = $criteria
}
Find-Item @props
```

### EXAMPLE 8

Find items within a data range. Possible filters are `InclusiveRange` and `ExclusiveRange` . When using dates, only **yyyyMMdd** is considered in the comparison so no need to get too precise.

```text
$props = @{
    Index = "sitecore_master_index"
    Criteria = @{
        Field = "__smallcreateddate"
        Filter = "InclusiveRange"
        Value = [datetime[]]@([datetime]"01/05/2015", [datetime]::Today)
    }, @{
        Field = "_name"
        Filter = "Fuzzy"
        Value = "sample"
    }
}

Find-Item @props
```

## Related Topics

* [Initialize-Item](initialize-item.md)
* Get-Item
* Get-ChildItem
* [https://gist.github.com/AdamNaj/273458beb3f2b179a0b6](https://gist.github.com/AdamNaj/273458beb3f2b179a0b6) 
* [https://weblogs.asp.net/scottgu/dynamic-linq-part-1-using-the-linq-dynamic-query-library](https://weblogs.asp.net/scottgu/dynamic-linq-part-1-using-the-linq-dynamic-query-library) 
* [https://github.com/SitecorePowerShell/Console/](https://github.com/SitecorePowerShell/Console/) 

