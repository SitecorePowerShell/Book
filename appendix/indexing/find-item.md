# Find-Item

Finds items using the Sitecore Content Search API.

## Syntax

Find-Item \[-Index\] &lt;String&gt; \[-Criteria &lt;SearchCriteria\[\]&gt;\] \[-Where &lt;String&gt;\] \[-WhereValues &lt;Object\[\]&gt;\] \[-OrderBy &lt;String&gt;\] \[-First &lt;Int32&gt;\] \[-Last &lt;Int32&gt;\] \[-Skip &lt;Int32&gt;\]

## Detailed Description

The Find-Item command searches for items using the Sitecore Content Search API.

© 2010-2017 Adam Najmanowicz, Michael West. All rights reserved. Sitecore PowerShell Extensions

## Parameters

### -Index  &lt;String&gt;

Name of the Index that will be used for the search:

Find-Item -Index sitecore\_master\_index -First 10

| Aliases |  |
| --- | --- | --- | --- | --- | --- |
| Required? | true |
| Position? | 1 |
| Default Value |  |
| Accept Pipeline Input? | false |
| Accept Wildcard Characters? | false |

### -Criteria  &lt;SearchCriteria\[\]&gt;

simple search criteria in the following example form:

@{ Filter = "Equals"; Field = "\_templatename"; Value = "PowerShell Script"; }, @{ Filter = "StartsWith"; Field = "\_fullpath"; Value = "/sitecore/system/Modules/PowerShell/Script Library/System Maintenance"; }, @{ Filter = "DescendantOf"; Value = \(Get-Item "master:/system/Modules/PowerShell/Script Library/"\) }

Where "Filter" is one of the following values:

* Equals
* StartsWith,
* Contains,
* EndsWith
* DescendantOf

Fields by which you can filter can be discovered using the following script:

Find-Item -Index sitecore\_master\_index `-Criteria @{Filter = "StartsWith"; Field = "_fullpath"; Value = "/sitecore/content/" }` -First 1 \| select -expand "Fields"

| Aliases |  |
| --- | --- | --- | --- | --- | --- |
| Required? | false |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | false |
| Accept Wildcard Characters? | false |

### -Where  &lt;String&gt;

Works on Sitecore 7.5 and later versions only.

Filtering Criteria using Dynamic Linq syntax: [https://weblogs.asp.net/scottgu/dynamic-linq-part-1-using-the-linq-dynamic-query-library](https://weblogs.asp.net/scottgu/dynamic-linq-part-1-using-the-linq-dynamic-query-library)

| Aliases |  |
| --- | --- | --- | --- | --- | --- |
| Required? | false |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | false |
| Accept Wildcard Characters? | false |

### -WhereValues  &lt;Object\[\]&gt;

Works on Sitecore 7.5 and later versions only.

An Array of objects for Dynamic Linq "-Where" parameter as explained in: [https://weblogs.asp.net/scottgu/dynamic-linq-part-1-using-the-linq-dynamic-query-library](https://weblogs.asp.net/scottgu/dynamic-linq-part-1-using-the-linq-dynamic-query-library)

| Aliases |  |
| --- | --- | --- | --- | --- | --- |
| Required? | false |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | false |
| Accept Wildcard Characters? | false |

### -OrderBy  &lt;String&gt;

Works on Sitecore 7.5 and later versions only.

Field by which the search results sorting should be performed. Dynamic Linq ordering syntax used. [https://weblogs.asp.net/scottgu/dynamic-linq-part-1-using-the-linq-dynamic-query-library](https://weblogs.asp.net/scottgu/dynamic-linq-part-1-using-the-linq-dynamic-query-library)

| Aliases |  |
| --- | --- | --- | --- | --- | --- |
| Required? | false |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | false |
| Accept Wildcard Characters? | false |

### -First  &lt;Int32&gt;

Number of returned search results.

| Aliases |  |
| --- | --- | --- | --- | --- | --- |
| Required? | false |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | false |
| Accept Wildcard Characters? | false |

### -Last  &lt;Int32&gt;

| Aliases |  |
| --- | --- | --- | --- | --- | --- |
| Required? | false |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | false |
| Accept Wildcard Characters? | false |

### -Skip  &lt;Int32&gt;

Number of search results to be skipped skip before returning the results commences.

| Aliases |  |
| --- | --- | --- | --- | --- | --- |
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

Fields by which filtering can be performed using the -Criteria parameter

```powershell
Find-Item -Index sitecore_master_index `
          -Criteria @{Filter = "StartsWith"; Field = "_fullpath"; Value = "/sitecore/content/" } `
          -First 1 | 
    select -expand "Fields"
```

### EXAMPLE 2

Find all children of a specific item including that item - return Sitecore items

```powershell
$root = (Get-Item "master:/system/Modules/PowerShell/Script Library/")
Find-Item -Index sitecore_master_index `
          -Criteria @{Filter = "DescendantOf"; Field = $root } |
    Initialize-Item
```

### EXAMPLE 3

Find all Template Fields using Dynamic LINQ syntax

```powershell
Find-Item `
    -Index sitecore_master_index `
    -Where 'TemplateName = @0 And Language=@1' `
    -WhereValues "Template Field", "en"
```

### EXAMPLE 4

Find all Template Fields using the -Criteria parameter syntax

```powershell
Find-Item `
        -Index sitecore_master_index `
        -Criteria @{Filter = "Equals"; Field = "_templatename"; Value = "Template Field"},
                  @{Filter = "Equals"; Field = "_language"; Value = "en"}
```

## Related Topics

* [Initialize-Item](initialize-item.md)
* Get-Item
* Get-ChildItem
* [https://gist.github.com/AdamNaj/273458beb3f2b179a0b6](https://gist.github.com/AdamNaj/273458beb3f2b179a0b6) 
* [https://weblogs.asp.net/scottgu/dynamic-linq-part-1-using-the-linq-dynamic-query-library](https://weblogs.asp.net/scottgu/dynamic-linq-part-1-using-the-linq-dynamic-query-library) 
* [https://github.com/SitecorePowerShell/Console/](https://github.com/SitecorePowerShell/Console/) 

