# Stop-SearchIndex

Stops the Sitecore index.

## Syntax

Stop-SearchIndex -Index &lt;ISearchIndex&gt;

Stop-SearchIndex \[-Name &lt;String&gt;\]

Stop-SearchIndex \[-Name &lt;String&gt;\]

## Detailed Description

The Stop-SearchIndex command stops the Sitecore index.

© 2010-2019 Adam Najmanowicz, Michael West. All rights reserved. Sitecore PowerShell Extensions

## Parameters

### -Index  &lt;ISearchIndex&gt;

| Aliases |  |
| :--- | :--- |
| Required? | true |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | true \(ByValue\) |
| Accept Wildcard Characters? | false |

### -Name  &lt;String&gt;

The name of the index to stop.

| Aliases |  |
| :--- | :--- |
| Required? | false |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | false |
| Accept Wildcard Characters? | false |

## Inputs

The input type is the type of the objects that you can pipe to the cmdlet.

* Sitecore.ContentSearch.ISearchIndex or System.String 

## Outputs

The output type is the type of the objects that the cmdlet emits.

* None 

## Notes

Help Author: Adam Najmanowicz, Michael West

## Examples

### EXAMPLE 1

```text
The following stops the indexing process from running.

PS master:\> Stop-SearchIndex -Name sitecore_master_index
```

### EXAMPLE 2

```text
The following stops the indexing process from running.

PS master:\> Get-SearchIndex -Name sitecore_master_index | Stop-SearchIndex
```

## Related Topics

* [Initialize-SearchIndex](initialize-searchindex.md)
* [Suspend-SearchIndex](suspend-searchindex.md)
* [Resume-SearchIndex](resume-searchindex.md)
* [Get-SearchIndex](get-searchindex.md)
* [https://github.com/SitecorePowerShell/Console/](https://github.com/SitecorePowerShell/Console/) 

