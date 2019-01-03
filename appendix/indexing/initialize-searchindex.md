# Initialize-SearchIndex

Rebuilds the Sitecore index.

## Syntax

Initialize-SearchIndex -Index &lt;ISearchIndex&gt; \[-IncludeRemoteIndex\] \[-AsJob\]

Initialize-SearchIndex \[-IncludeRemoteIndex\] \[-Name &lt;String&gt;\] \[-AsJob\]

Initialize-SearchIndex \[-Name &lt;String&gt;\] \[-AsJob\]

## Detailed Description

The Rebuild-SearchIndex command rebuilds Sitecore index. This command is an alias for Initialize-SearchIndex.

© 2010-2019 Adam Najmanowicz, Michael West. All rights reserved. Sitecore PowerShell Extensions\#\# Aliases

The following abbreviations are aliases for this cmdlet:

* Rebuild-SearchIndex 

## Parameters

### -Index  &lt;ISearchIndex&gt;

The index instance.

| Aliases |  |
| :--- | :--- |
| Required? | true |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | true \(ByValue, ByPropertyName\) |
| Accept Wildcard Characters? | false |

### -IncludeRemoteIndex  &lt;SwitchParameter&gt;

The remote indexing should be triggered.

| Aliases |  |
| :--- | :--- |
| Required? | false |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | false |
| Accept Wildcard Characters? | false |

### -AsJob  &lt;SwitchParameter&gt;

The job created for rebuilding the index should be returned as output.

| Aliases |  |
| :--- | :--- |
| Required? | false |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | false |
| Accept Wildcard Characters? | false |

### -Name  &lt;String&gt;

The name of the index to resume.

| Aliases |  |
| :--- | :--- |
| Required? | false |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | false |
| Accept Wildcard Characters? | false |

## Inputs

The input type is the type of the objects that you can pipe to the cmdlet.

* None or Sitecore.Jobs.Job 

## Outputs

The output type is the type of the objects that the cmdlet emits.

* None 

## Notes

Help Author: Adam Najmanowicz, Michael West

## Examples

### EXAMPLE 1

```text
The following rebuilds the index.

PS master:\> Rebuild-SearchIndex -Name sitecore_master_index
```

### EXAMPLE 2

```text
The following rebuilds the index.

PS master:\> Get-SearchIndex -Name sitecore_master_index | Rebuild-SearchIndex
```

## Related Topics

* [Resume-SearchIndex](resume-searchindex.md)
* [Suspend-SearchIndex](suspend-searchindex.md)
* [Stop-SearchIndex](stop-searchindex.md)
* [Get-SearchIndex](get-searchindex.md)
* [https://github.com/SitecorePowerShell/Console/](https://github.com/SitecorePowerShell/Console/) 

