# Get-ItemReferrer

Returns all the items referring to the specified item.

## Syntax

Get-ItemReferrer -Item &lt;Item&gt;

Get-ItemReferrer -Item &lt;Item&gt; -ItemLink

Get-ItemReferrer -Path &lt;String&gt; \[-Language &lt;String\[\]&gt;\]

Get-ItemReferrer -Path &lt;String&gt; \[-Language &lt;String\[\]&gt;\] -ItemLink

Get-ItemReferrer -Id &lt;String&gt; \[-Database &lt;String&gt;\] \[-Language &lt;String\[\]&gt;\]

Get-ItemReferrer -Id &lt;String&gt; \[-Database &lt;String&gt;\] \[-Language &lt;String\[\]&gt;\] -ItemLink

## Detailed Description

The Get-ItemReferrer command returns all items referring to the specified item. If -ItemLink parameter is used the command will return links rather than items.

© 2010-2019 Adam Najmanowicz, Michael West. All rights reserved. Sitecore PowerShell Extensions

## Parameters

### -Item  &lt;Item&gt;

The item to be analysed.

| Aliases |  |
| :--- | :--- |
| Required? | true |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | true \(ByValue, ByPropertyName\) |
| Accept Wildcard Characters? | false |

### -Path  &lt;String&gt;

Path to the item to be processed - additionally specify Language parameter to fetch different item language than the current user language.

| Aliases |  |
| :--- | :--- |
| Required? | true |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | false |
| Accept Wildcard Characters? | false |

### -Id  &lt;String&gt;

Id of the the item to be processed - additionally specify Language parameter to fetch different item language than the current user language.

| Aliases |  |
| :--- | :--- |
| Required? | true |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | false |
| Accept Wildcard Characters? | false |

### -Database  &lt;String&gt;

Database containing the item to be processed - can work with Language parameter to narrow the publication scope.

| Aliases |  |
| :--- | :--- |
| Required? | false |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | false |
| Accept Wildcard Characters? | false |

### -Language  &lt;String\[\]&gt;

Language that will be used as source language. If not specified the current user language will be used. Globbing/wildcard supported.

| Aliases |  |
| :--- | :--- |
| Required? | false |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | false |
| Accept Wildcard Characters? | false |

### -ItemLink  &lt;SwitchParameter&gt;

Return ItemLink that define both source and target of a link rather than items linking to the specified item.

| Aliases |  |
| :--- | :--- |
| Required? | true |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | false |
| Accept Wildcard Characters? | false |

## Inputs

The input type is the type of the objects that you can pipe to the cmdlet.

* Sitecore.Data.Items.Item 

## Outputs

The output type is the type of the objects that the cmdlet emits.

* Sitecore.Data.Items.Item

  Sitecore.Links.ItemLink

## Notes

Help Author: Adam Najmanowicz, Michael West

## Examples

### EXAMPLE 1

```text
PS master:\>Get-ItemReferrer -Path master:\content\home

Name                             Children Languages                Id                                     TemplateName
----                             -------- ---------                --                                     ------------
Home                             True     {en, de-DE, es-ES, pt... {110D559F-DEA5-42EA-9C1C-8A5DF7E70EF9} Sample Item
Form                             False    {en, de-DE, es-ES, pt... {6D3B4E7D-FEF8-4110-804A-B56605688830} Webcontrol
news                             True     {en, de-DE, es-ES, pt... {DB894F2F-D53F-4A2D-B58F-957BFAC2C848} Article
learn-about-oms                  False    {en, de-DE, es-ES, pt... {79ECF4DF-9DB7-430F-9BFF-D164978C2333} Link
```

### EXAMPLE 2

```text
PS master:\>Get-Item master:\content\home | Get-ItemReferrer -ItemLink

SourceItemLanguage : en
SourceItemVersion  : 1
TargetItemLanguage :
TargetItemVersion  : 0
SourceDatabaseName : master
SourceFieldID      : {F685964D-02E1-4DB6-A0A2-BFA59F5F9806}
SourceItemID       : {110D559F-DEA5-42EA-9C1C-8A5DF7E70EF9}
TargetDatabaseName : master
TargetItemID       : {110D559F-DEA5-42EA-9C1C-8A5DF7E70EF9}
TargetPath         : /sitecore/content/Home
```

## Related Topics

* [Get-ItemReference](get-itemreference.md)
* [Update-ItemReferrer](update-itemreferrer.md)
* [https://github.com/SitecorePowerShell/Console/](https://github.com/SitecorePowerShell/Console/) 

