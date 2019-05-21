# Get-SearchIndex

Returns the available Sitecore indexes.

## Syntax

Get-SearchIndex \[-Name &lt;String&gt;\]

Get-SearchIndex \[-Name &lt;String&gt;\]

## Detailed Description

The Get-SearchIndex command returns the available Sitecore indexes. These are the same as those found in the Control Panel.

© 2010-2019 Adam Najmanowicz, Michael West. All rights reserved. Sitecore PowerShell Extensions

## Parameters

### -Name  &lt;String&gt;

Name of the index to return.

| Aliases |  |
| :--- | :--- |
| Required? | false |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | false |
| Accept Wildcard Characters? | false |

## Inputs

The input type is the type of the objects that you can pipe to the cmdlet.

* None or System.String 

## Outputs

The output type is the type of the objects that the cmdlet emits.

* Sitecore.ContentSearch.ISearchIndex 

## Notes

Help Author: Adam Najmanowicz, Michael West

## Examples

### EXAMPLE 1

```text
The following lists all available indexes.

PS master:\>Get-SearchIndex

Name                             IndexingState   IsRebuilding    IsSharded
----                             -------------   ------------    ---------
sitecore_analytics_index         Started         False           False
sitecore_core_index              Started         False           False
sitecore_master_index            Started         True            False
sitecore_web_index               Started         False           False
sitecore_marketing_asset_inde... Started         False           False
sitecore_marketing_asset_inde... Started         False           False
sitecore_testing_index           Started         False           False
sitecore_suggested_test_index    Started         False           False
sitecore_fxm_master_index        Started         False           False
sitecore_fxm_web_index           Started         False           False
sitecore_list_index              Started         False           False
social_messages_master           Started         False           False
social_messages_web              Started         False           False
```

### EXAMPLE 2

```text
The following lists only the specified index.

PS master:\>Get-SearchIndex -Name sitecore_master_index

Name                             IndexingState   IsRebuilding    IsSharded
----                             -------------   ------------    ---------
sitecore_master_index            Started         True            False
```

## Related Topics

* [Initialize-SearchIndex](initialize-searchindex.md)
* [Stop-SearchIndex](stop-searchindex.md)
* [Resume-SearchIndex](resume-searchindex.md)
* [Suspend-SearchIndex](suspend-searchindex.md)
* [https://github.com/SitecorePowerShell/Console/](https://github.com/SitecorePowerShell/Console/) 

