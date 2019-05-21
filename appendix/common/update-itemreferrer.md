# Update-ItemReferrer

Updates references to specified item to point to a new provided in the -NewTarget ore removes links to the item.

## Syntax

Update-ItemReferrer \[-Link\] &lt;ItemLink&gt; \[-NewTarget\] &lt;Item&gt;

Update-ItemReferrer \[-Link\] &lt;ItemLink&gt; -RemoveLink

Update-ItemReferrer \[-NewTarget\] &lt;Item&gt; \[-Item\] &lt;Item&gt;

Update-ItemReferrer -NewTarget &lt;Item&gt; -Path &lt;String&gt; \[-Language &lt;String\[\]&gt;\]

Update-ItemReferrer -NewTarget &lt;Item&gt; -Id &lt;String&gt; \[-Database &lt;String&gt;\] \[-Language &lt;String\[\]&gt;\]

Update-ItemReferrer -RemoveLink \[-Item\] &lt;Item&gt;

Update-ItemReferrer -RemoveLink -Path &lt;String&gt; \[-Language &lt;String\[\]&gt;\]

Update-ItemReferrer -RemoveLink -Id &lt;String&gt; \[-Database &lt;String&gt;\] \[-Language &lt;String\[\]&gt;\]

## Detailed Description

The cmdlet manipulates link to a specific item. The target item can be provided as an Item object or through Path/ID. it does not modifies the item itself but rather other items that link to it. If the -RemoveLink parameter is used the link will be removed rather than modified. To deliver more fine grained filtering you can provide ItemLink using the -Link parameter. You can obtain ItemLinks using Get-ItemReferrer or Get-ItemReference cmdlets. Consult Examples for specific use cases of each approach.

© 2010-2019 Adam Najmanowicz, Michael West. All rights reserved. Sitecore PowerShell Extensions

## Parameters

### -Link  &lt;ItemLink&gt;

ItemLink retrieved from the Link database. Use this parameter to do more granular filtering.

| Aliases |  |
| :--- | :--- |
| Required? | true |
| Position? | 1 |
| Default Value |  |
| Accept Pipeline Input? | true \(ByValue\) |
| Accept Wildcard Characters? | false |

### -NewTarget  &lt;Item&gt;

New item the links should be pointing to

| Aliases |  |
| :--- | :--- |
| Required? | true |
| Position? | 2 |
| Default Value |  |
| Accept Pipeline Input? | false |
| Accept Wildcard Characters? | false |

### -RemoveLink  &lt;SwitchParameter&gt;

If provided, removes all links to the current target item.

| Aliases |  |
| :--- | :--- |
| Required? | true |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | false |
| Accept Wildcard Characters? | false |

### -Item  &lt;Item&gt;

The current item to be relinked.

| Aliases |  |
| :--- | :--- |
| Required? | true |
| Position? | 1 |
| Default Value |  |
| Accept Pipeline Input? | true \(ByValue, ByPropertyName\) |
| Accept Wildcard Characters? | false |

### -Path  &lt;String&gt;

Path to the current item to be relinked - can work with Language parameter to narrow the publication scope.

| Aliases |  |
| :--- | :--- |
| Required? | true |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | false |
| Accept Wildcard Characters? | false |

### -Id  &lt;String&gt;

Id of the current item to be relinked - can work with Language parameter to specify the language other than current session language. Requires the Database parameter to be specified.

| Aliases |  |
| :--- | :--- |
| Required? | true |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | false |
| Accept Wildcard Characters? | false |

### -Database  &lt;String&gt;

Database containing the current item to be relinked.

| Aliases |  |
| :--- | :--- |
| Required? | false |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | false |
| Accept Wildcard Characters? | false |

### -Language  &lt;String\[\]&gt;

If you need the current item to be relinked in specific Language You can specify it with this parameter. Globbing/wildcard supported.

| Aliases |  |
| :--- | :--- |
| Required? | false |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | false |
| Accept Wildcard Characters? | false |

## Inputs

The input type is the type of the objects that you can pipe to the cmdlet.

* Sitecore.Data.Items.Item 

## Notes

Help Author: Adam Najmanowicz, Michael West

## Examples

### EXAMPLE 1

This example covers global operations

Assuming Sitecore PowerShell Extensions 4.2 or newer is installed Assuming your Home has an "Image" field of type "Image" Assuming you have second item next to Home called Home2 that has an "Image" field of type "Image"

```text
$coverImage = Get-Item 'master:\media library\Default Website\cover'
$scLogoImage = Get-item 'master:\media library\Default Website\sc_logo'

Write-Host "`nReset 'home', 'child' and 'home2' to link to 'cover'- 3 items" -Foreground Magenta
(Get-item master:\content\home).Image = $coverImage
(Get-item master:\content\Home\child).Image = $coverImage
(Get-item master:\content\home2).Image = $coverImage

Get-ItemReferrer -Item $coverImage            

Write-Host "`nRelinking all instances of 'cover' image to  'sc_logo'" -Foreground Yellow
$coverImage | Update-ItemReferrer -NewTarget $scLogoImage

Write-Host "`n'cover' should no longer have links leading to it - 0 items " -Foreground Red
$coverImage | Get-ItemReferrer 

Write-Host "`n'sc_logo' should now be linked from all - 3 items" -Foreground Green
$scLogoImage | Get-ItemReferrer

Write-Host "`nRemoving links to 'sc_logo' from all items" -Foreground Yellow
$scLogoImage | Update-ItemReferrer -RemoveLink

Write-Host "`n'sc_logo' should have no links to it - 0 items" -Foreground Red
$scLogoImage | Get-ItemReferrer
```

### EXAMPLE 2

This example covers more fine-grained filtered approach to removing links

Assuming Sitecore PowerShell Extensions 4.2 or newer is installed Assuming your Home has an "Image" field of type "Image" Assuming you have second item next to Home called Home2 that has an "Image" field of type "Image"

```text
$coverImage = Get-Item 'master:\media library\Default Website\cover'
$scLogoImage = Get-item 'master:\media library\Default Website\sc_logo'

Write-Host "`nReset 'home', 'child' and 'home2' to link to 'cover'- 3 items" -Foreground Magenta
(Get-item master:\content\home).Image = $coverImage
(Get-item master:\content\Home\child).Image = $coverImage
(Get-item master:\content\home2).Image = $coverImage

Get-ItemReferrer -Item $coverImage            

Write-Host "`nRemove links to the 'cover' image from all items under 'master:\content\home'" -Foreground Yellow
Get-ChildItem master:\content\home -WithParent -Recurse |   # get items under home
    Get-ItemReference -ItemLink |                           # get all items that they are refering to
    ? { $_.TargetItemID -eq $coverImage.ID } |              # filter only references to $coverImage
    Update-ItemReferrer -RemoveLink                         # remove links

Write-Host "`n'cover' should have 1 link leading from 'home2'" -Foreground Green
$coverImage | Get-ItemReferrer
```

### EXAMPLE 3

This example covers more fine-grained filtered approach to removing links

Assuming Sitecore PowerShell Extensions 4.2 or newer is installed Assuming your Home has an "Image" field of type "Image" Assuming you have second item next to Home called Home2 that has an "Image" field of type "Image"

```text
$coverImage = Get-Item 'master:\media library\Default Website\cover'
$scLogoImage = Get-item 'master:\media library\Default Website\sc_logo'

Write-Host "`nReset 'home', 'child' and 'home2' to link to 'cover'- 3 items" -Foreground Magenta
(Get-item master:\content\home).Image = $coverImage
(Get-item master:\content\Home\child).Image = $coverImage
(Get-item master:\content\home2).Image = $coverImage

Get-ItemReferrer -Item $coverImage            

Write-Host "`nUpdate all links to 'cover' image to point to 'sc_logo' from all immediate children of /sitecore/content" -Foreground Yellow
Get-ChildItem master:\content |                     # get items immediately under 'under home'content'
    Get-ItemReference -ItemLink |                   # get all items that they are refering to
    ? { $_.TargetItemID -eq $coverImage.ID } |      # filter only references to $coverImage
    Update-ItemReferrer -NewTarget $scLogoImage     # point them to 'sc_logo' image

Write-Host "`n'cover' should have link from home2/child - 1 item" -Foreground Green
$coverImage | Get-ItemReferrer

Write-Host "`n'sc_logo' should have links leading from 'home' and 'home2' - 2 items" -Foreground Green
$scLogoImage | Get-ItemReferrer
```

## Related Topics

* [Get-ItemReferrer](get-itemreferrer.md)
* [Get-ItemReference](get-itemreference.md)
* [https://github.com/SitecorePowerShell/Console/](https://github.com/SitecorePowerShell/Console/) 

