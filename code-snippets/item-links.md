---
description: Examples for managing item referrers maintained by the Link Database.
---

# Item Links

### Relink Item

**Example:** The following changes the image linked on an item to a new image. Originally posted [here](https://gist.github.com/michaellwest/f563b0b3597f6c0a75d6).

```powershell
$item = Get-Item -Path "master:\media library\images\koala"
$itemNew = Get-Item -Path "master:\media library\images\penguins"
$links = Get-ItemReferrer -Item $item -ItemLink
foreach($link in $links) {
    $linkedItem = Get-Item -Path master:\ -ID $link.SourceItemID 
    $itemField = $linkedItem.Fields[$link.SourceFieldID]
    $field = [Sitecore.Data.Fields.FieldTypeManager]::GetField($itemField)
    
    $linkedItem.Editing.BeginEdit()
    $field.Relink($link, $itemNew)
    $linkedItem.Editing.EndEdit() | Out-Null
}
```

### Remove Item Link

Example: The following removes an item link followed by removing the item. Originally posted [here](https://gist.github.com/michaellwest/f563b0b3597f6c0a75d6).

```powershell
# Crafted by Dylan

function Remove-ItemLink {
    param([Item]$item)
    
    $linkDb = [Sitecore.Globals]::LinkDatabase

    $links = Get-ItemReferrer -Item $item -ItemLink

    foreach($link in $links) {
        $linkedItem = Get-Item -Path master:\ -ID $link.SourceItemID 
        $itemField = $linkedItem.Fields[$link.SourceFieldID]
        $field = [Sitecore.Data.Fields.FieldTypeManager]::GetField($itemField)

        $linkedItem.Editing.BeginEdit()
        $field.RemoveLink($link)
        $linkedItem.Editing.EndEdit()
    }
}

# Example usage: delete items along with their references that have passed a certain date defined by a 'date' field
$today = Get-Date
$todayIsoDate = [Sitecore.DateUtil]::ToIsoDate($today)
$query = "/sitecore/system/Modules/Mysite/Service Schedules/*[@date < '$($todayIsoDate)']"
$itemsToDelete = Get-Item -Path master: -Query $query

foreach($item in $itemsToDelete) {
    Write-Host "Cleaning up $($itemsToDelete.Paths.Path)"
    Remove-ItemLink -Item $item
    Remove-Item -Path $item.Paths.Path
}
```

