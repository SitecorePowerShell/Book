# Archiving Items

Sitecore provides archive functionality to manage deleted and expired content. This page covers how to work with Sitecore archives using SPE cmdlets.

## Understanding Sitecore Archives

Sitecore maintains two default archives for each database:

- **recyclebin** - Temporary storage for deleted items (similar to Windows Recycle Bin)
- **archive** - Long-term archive for items scheduled for automatic archival

Archives preserve complete item information including field values, versions, and metadata, allowing items to be restored when needed.

## Archive vs. Delete vs. Recycle

Understanding the different ways items can be removed:

| Operation        | Command                    | Storage                        | Reversible                     | Use Case                               |
| ---------------- | -------------------------- | ------------------------------ | ------------------------------ | -------------------------------------- |
| Recycle          | `Remove-Item`              | Moved to recyclebin            | Yes, via `Restore-ArchiveItem` | Accidental deletion, temporary removal |
| Permanent Delete | `Remove-Item -Permanently` | Deleted from database          | No                             | Final cleanup, no recovery needed      |
| Archive          | `Remove-Item -Archive`     | Moved to archive automatically | Yes, via `Restore-ArchiveItem` | On-demand archival                     |
| Purge Archive    | `Remove-ArchiveItem`       | Deleted from archive           | No                             | Cleanup old archived items             |

{% hint style="info" %}
By default, `Remove-Item` sends items to the recycle bin. Use `-Permanently` only when you're certain the item should never be recovered.
{% endhint %}

## Working with the Recycle Bin

The recycle bin provides a safety net for deleted items.

### Viewing Recycle Bin Contents

**Example:** List all items in the recycle bin.

```powershell
$database = Get-Database -Name "master"
$recyclebin = Get-Archive -Database $database -Name "recyclebin"
$items = Get-ArchiveItem -Archive $recyclebin

$items | Select-Object OriginalLocation, ArchiveDate, ArchivedBy | Format-Table
```

**Output:**

```powershell
OriginalLocation                    ArchiveDate          ArchivedBy
----------------                    -----------          ----------
/sitecore/content/home/old-page     12/15/2024 10:30:00  sitecore\admin
/sitecore/content/home/test-item    12/14/2024 14:22:00  sitecore\editor
```

### Restoring from Recycle Bin

**Example:** Restore a specific item by ID.

```powershell
$database = Get-Database -Name "master"
$recyclebin = Get-Archive -Database $database -Name "recyclebin"
$itemId = "{1BB32980-66B4-4ADA-9170-10A9D3336613}"

Restore-ArchiveItem -Archive $recyclebin -ItemId $itemId
```

**Example:** Restore items deleted recently.

```powershell
$database = Get-Database -Name "master"
$recyclebin = Get-Archive -Database $database -Name "recyclebin"
$yesterdayDate = (Get-Date).AddDays(-1)

Get-ArchiveItem -Archive $recyclebin |
    Where-Object { $_.ArchiveDate -gt $yesterdayDate } |
    Restore-ArchiveItem
```

**Example:** Restore items from a specific path.

```powershell
$database = Get-Database -Name "master"
$recyclebin = Get-Archive -Database $database -Name "recyclebin"

Get-ArchiveItem -Archive $recyclebin |
    Where-Object { $_.OriginalLocation -like "/sitecore/content/home/*" } |
    Restore-ArchiveItem
```

### Finding Items by User

**Example:** Find items deleted by a specific user.

```powershell
$database = Get-Database -Name "master"
$recyclebin = Get-Archive -Database $database -Name "recyclebin"

Get-ArchiveItem -Archive $recyclebin -Identity "sitecore\admin"
```

**Example:** Restore all items deleted by a user.

```powershell
$database = Get-Database -Name "master"
$recyclebin = Get-Archive -Database $database -Name "recyclebin"

Get-ArchiveItem -Archive $recyclebin -Identity "sitecore\editor" |
    Restore-ArchiveItem
```

### Cleaning Up the Recycle Bin

**Example:** Permanently delete old recycle bin items.

```powershell
$database = Get-Database -Name "master"
$recyclebin = Get-Archive -Database $database -Name "recyclebin"
$cutoffDate = (Get-Date).AddDays(-30)

Get-ArchiveItem -Archive $recyclebin |
    Where-Object { $_.ArchiveDate -lt $cutoffDate } |
    Remove-ArchiveItem
```

{% hint style="danger" %}
`Remove-ArchiveItem` permanently deletes items from the archive. This operation cannot be undone. Always verify items before removal.
{% endhint %}

**Example:** Clean up with user confirmation.

```powershell
$database = Get-Database -Name "master"
$recyclebin = Get-Archive -Database $database -Name "recyclebin"
$cutoffDate = (Get-Date).AddDays(-30)

$itemsToRemove = Get-ArchiveItem -Archive $recyclebin |
    Where-Object { $_.ArchiveDate -lt $cutoffDate }

Write-Host "Found $($itemsToRemove.Count) items older than 30 days"

$response = Show-Confirm -Title "Permanently delete these $($itemsToRemove.Count) items from the recycle bin?"

if ($response -eq 'yes') {
    $itemsToRemove | Remove-ArchiveItem
    Write-Host "Deleted $($itemsToRemove.Count) items from recycle bin" -ForegroundColor Green
}
```

## Working with the Archive

The archive is used for scheduled content expiration based on the `__Archive date` field.

### Setting Archive Dates

**Example:** Set an item to be archived in the future.

```powershell
$item = Get-Item -Path "master:\content\home\old-campaign"
$archiveDate = (Get-Date).AddDays(90)

# Convert to Sitecore ISO format
$utcTime = [Sitecore.DateUtil]::ToUniversalTime($archiveDate)
$isoDate = [Sitecore.DateUtil]::ToIsoDate($utcTime)

$item.Editing.BeginEdit()
$item[[Sitecore.FieldIDs]::ArchiveDate] = $isoDate
$item.Editing.EndEdit()

Write-Host "Item will be archived on: $($archiveDate.ToShortDateString())"
```

{% hint style="info" %}
Sitecore's archive agent runs periodically to move items with past archive dates to the archive. The item must exist in the database until the archive date passes and the agent runs.
{% endhint %}

### Retrieving Archived Items

**Example:** View all archived items.

```powershell
$database = Get-Database -Name "master"
$archive = Get-Archive -Database $database -Name "archive"

Get-ArchiveItem -Archive $archive |
    Select-Object OriginalLocation, ArchiveDate |
    Format-Table
```

**Example:** Find a specific archived item.

```powershell
$database = Get-Database -Name "master"
$archive = Get-Archive -Database $database -Name "archive"
$itemId = "{9459ADDD-4471-4ED3-A041-D33E559BD321}"

$archivedItem = Get-ArchiveItem -Archive $archive -ItemId $itemId

if ($archivedItem) {
    Write-Host "Found archived item: $($archivedItem.OriginalLocation)"
    Write-Host "Archived on: $($archivedItem.ArchiveDate)"
}
```

### Restoring Archived Items

**Example:** Restore an item from the archive.

```powershell
$database = Get-Database -Name "master"
$archive = Get-Archive -Database $database -Name "archive"
$itemId = "{9459ADDD-4471-4ED3-A041-D33E559BD321}"

Restore-ArchiveItem -Archive $archive -ItemId $itemId
```

**Example:** Review and selectively restore items.

```powershell
$database = Get-Database -Name "master"
$archive = Get-Archive -Database $database -Name "archive"

$archivedItems = Get-ArchiveItem -Archive $archive |
    Where-Object { $_.OriginalLocation -like "*\content\campaigns\*" }

# Use ListView for interactive selection
$archivedItems |
    Show-ListView -Property OriginalLocation, ArchiveDate, ArchivedBy |
    Restore-ArchiveItem
```

## Common Patterns

### Pattern: Interactive Recycle Bin Management

```powershell
$database = Get-Database -Name "master"
$recyclebin = Get-Archive -Database $database -Name "recyclebin"

$items = Get-ArchiveItem -Archive $recyclebin

$props = @{
    Property = "OriginalLocation", "ArchiveDate", "ArchivedBy"
    Title = "Recycle Bin Items"
    InfoTitle = "Select items to restore"
    InfoDescription = "Choose one or more items to restore to their original locations"
}

$selectedItems = $items | Show-ListView @props

if ($selectedItems) {
    $selectedItems | Restore-ArchiveItem
    Write-Host "Restored $($selectedItems.Count) items" -ForegroundColor Green
}
```

### Pattern: Audit Archive Activity

```powershell
$database = Get-Database -Name "master"
$recyclebin = Get-Archive -Database $database -Name "recyclebin"

# Generate audit report
$report = Get-ArchiveItem -Archive $recyclebin |
    Group-Object ArchivedBy |
    Select-Object @{Name="User"; Expression={$_.Name}},
                  @{Name="ItemsDeleted"; Expression={$_.Count}}

$report | Format-Table -AutoSize
```

**Output:**

```powershell
User              ItemsDeleted
----              ------------
sitecore\admin              25
sitecore\editor             12
sitecore\author              8
```

### Pattern: Scheduled Recycle Bin Cleanup

```powershell
# This script can be scheduled as a Sitecore task
$database = Get-Database -Name "master"
$recyclebin = Get-Archive -Database $database -Name "recyclebin"
$retentionDays = 30
$cutoffDate = (Get-Date).AddDays(-$retentionDays)

$itemsToRemove = Get-ArchiveItem -Archive $recyclebin |
    Where-Object { $_.ArchiveDate -lt $cutoffDate }

if ($itemsToRemove) {
    $count = $itemsToRemove.Count
    Write-Log "Cleaning up $count items from recycle bin older than $retentionDays days"

    $itemsToRemove | Remove-ArchiveItem

    Write-Log "Successfully removed $count items from recycle bin"
} else {
    Write-Log "No items found in recycle bin older than $retentionDays days"
}
```

### Pattern: Bulk Archive Date Setting

```powershell
# Archive all items from an old campaign
$campaignPath = "master:\content\home\campaigns\2023"
$archiveDate = (Get-Date).AddDays(30)

$utcTime = [Sitecore.DateUtil]::ToUniversalTime($archiveDate)
$isoDate = [Sitecore.DateUtil]::ToIsoDate($utcTime)

Get-ChildItem -Path $campaignPath -Recurse |
    ForEach-Object {
        $_.Editing.BeginEdit()
        $_[[Sitecore.FieldIDs]::ArchiveDate] = $isoDate
        $_.Editing.EndEdit() > $null
        Write-Host "Set archive date for: $($_.ItemPath)"
    }

Write-Host "Campaign items will be archived on: $($archiveDate.ToShortDateString())" -ForegroundColor Green
```

### Pattern: Restore with Conflict Detection

```powershell
$database = Get-Database -Name "master"
$recyclebin = Get-Archive -Database $database -Name "recyclebin"

$archivedItems = Get-ArchiveItem -Archive $recyclebin

foreach ($archivedItem in $archivedItems) {
    # Check if an item with the same path already exists
    $existingItem = Get-Item -Path $archivedItem.OriginalLocation -ErrorAction SilentlyContinue

    if ($existingItem) {
        Write-Host "Conflict: Item already exists at $($archivedItem.OriginalLocation)" -ForegroundColor Yellow
    } else {
        Restore-ArchiveItem -ArchiveItem $archivedItem
        Write-Host "Restored: $($archivedItem.OriginalLocation)" -ForegroundColor Green
    }
}
```

### Pattern: Export Archive Report

```powershell
$database = Get-Database -Name "master"
$recyclebin = Get-Archive -Database $database -Name "recyclebin"

$report = Get-ArchiveItem -Archive $recyclebin |
    Select-Object @{Name="Path"; Expression={$_.OriginalLocation}},
                  @{Name="DeletedDate"; Expression={$_.ArchiveDate}},
                  @{Name="DeletedBy"; Expression={$_.ArchivedBy}},
                  @{Name="ArchivalId"; Expression={$_.ArchivalId}}

$report | Export-Csv "$($SitecoreDataFolder)\export\recyclebin-report.csv" -NoTypeInformation

Write-Host "Exported report with $($report.Count) items" -ForegroundColor Green
```

## Archive Administration

### Checking Archive Statistics

**Example:** Get archive sizes and item counts.

```powershell
$database = Get-Database -Name "master"
$archives = Get-Archive -Database $database

foreach ($archive in $archives) {
    $itemCount = (Get-ArchiveItem -Archive $archive | Measure-Object).Count

    Write-Host "$($archive.Name): $itemCount items" -ForegroundColor Cyan
}
```

**Output:**

```powershell
archive: 125 items
recyclebin: 1,842 items
```

### Working with Multiple Databases

**Example:** Clean up recycle bins across all databases.

```powershell
$databases = Get-Database | Where-Object { $_.Name -in @("master", "web", "core") }
$cutoffDate = (Get-Date).AddDays(-30)

foreach ($database in $databases) {
    Write-Host "`nProcessing database: $($database.Name)" -ForegroundColor Cyan

    $recyclebin = Get-Archive -Database $database -Name "recyclebin"
    $oldItems = Get-ArchiveItem -Archive $recyclebin |
        Where-Object { $_.ArchiveDate -lt $cutoffDate }

    if ($oldItems) {
        Write-Host "  Found $($oldItems.Count) old items to remove"
        $oldItems | Remove-ArchiveItem
        Write-Host "  Cleanup complete" -ForegroundColor Green
    } else {
        Write-Host "  No items to remove"
    }
}
```

## Best Practices

### Regular Maintenance

- **Schedule cleanup** - Automate recycle bin cleanup to prevent database bloat
- **Retention policy** - Define how long items stay in recycle bin before permanent deletion
- **Monitor size** - Track archive sizes as part of database maintenance

### Safety and Recovery

- **Backup first** - Always backup the database before permanently removing archived items
- **Review before deletion** - Use `Show-ListView` to review items before permanent removal
- **Test restores** - Periodically test restoration process in lower environments
- **Audit trails** - Export archive reports for compliance and audit purposes

### Archive Date Management

- **Consistent dates** - Use archive dates for planned content expiration (campaigns, promotions)
- **Grace periods** - Set archive dates with buffer time for review
- **Bulk operations** - Set archive dates programmatically for content batches
- **Time zones** - Always convert to UTC using `[Sitecore.DateUtil]::ToUniversalTime()`

### Performance Considerations

- **Limit scope** - Use `-ItemId` or `-Identity` parameters when possible instead of retrieving all items
- **Batch operations** - Process large archive cleanups during maintenance windows
- **Progress reporting** - Provide feedback for long-running archive operations

**Example:** Efficient cleanup with progress reporting.

```powershell
$database = Get-Database -Name "master"
$recyclebin = Get-Archive -Database $database -Name "recyclebin"
$cutoffDate = (Get-Date).AddDays(-90)

$itemsToRemove = @(Get-ArchiveItem -Archive $recyclebin |
    Where-Object { $_.ArchiveDate -lt $cutoffDate })

$total = $itemsToRemove.Count
$current = 0

Write-Host "Removing $total items from recycle bin..."

foreach ($item in $itemsToRemove) {
    $current++

    if ($current % 100 -eq 0) {
        Write-Progress -Activity "Cleaning recycle bin" `
            -Status "Removed $current of $total items" `
            -PercentComplete (($current / $total) * 100)
    }

    Remove-ArchiveItem -ArchiveItem $item
}

Write-Progress -Activity "Cleaning recycle bin" -Completed
Write-Host "Cleanup complete: $total items removed" -ForegroundColor Green
```

## Important Considerations

### Archive Behavior

- **Preservation** - Archived items retain all versions, field values, and metadata
- **Parent structure** - Restoring an item without its parent may require manual placement
- **Security** - Users must have appropriate permissions to restore items
- **Events** - Archive operations (restore, remove) do not trigger item events or workflows

### Database Impact

- **Size growth** - Large recycle bins can significantly increase database size
- **Query performance** - Very large archives may slow archive queries
- **Cleanup schedule** - Regular cleanup prevents performance degradation

### Edge Cases

- **Duplicate names** - Restoring when an item with the same name exists at the original location
- **Missing parents** - Restoring items whose parent has been permanently deleted
- **Template changes** - Restoring items when the template definition has changed
- **Multiple versions** - Archive stores all item versions; restore brings back all versions

## Troubleshooting

### Common Issues

**Problem:** Cannot restore item - parent doesn't exist

```powershell
# Solution: Check if parent exists, create if needed
$archivedItem = Get-ArchiveItem -Archive $recyclebin -ItemId $itemId
$parentPath = Split-Path $archivedItem.OriginalLocation -Parent

$parent = Get-Item -Path "master:$parentPath" -ErrorAction SilentlyContinue

if (-not $parent) {
    Write-Host "Parent does not exist: $parentPath" -ForegroundColor Red
    # Either create parent or restore to a different location
}
```

**Problem:** Archive query is slow

```powershell
# Solution: Use specific parameters instead of retrieving all items
# Slow
$items = Get-ArchiveItem -Archive $recyclebin | Where-Object { $_.ArchivedBy -eq "sitecore\admin" }

# Faster
$items = Get-ArchiveItem -Archive $recyclebin -Identity "sitecore\admin"
```

## See Also

- [Creating and Removing Items](creating-and-removing-items.md) - Item creation and deletion fundamentals
- [Get-Archive](../appendix/common/get-archive.md) - Cmdlet reference
- [Get-ArchiveItem](../appendix/common/get-archiveitem.md) - Cmdlet reference
- [Restore-ArchiveItem](../appendix/common/restore-archiveitem.md) - Cmdlet reference
- [Remove-ArchiveItem](../appendix/common/remove-archiveitem.md) - Cmdlet reference
- [Best Practices](best-practices.md) - Performance optimization and patterns

## References

- [Sitecore Archives Documentation](https://doc.sitecore.com/xp/en/developers/latest/platform-administration-and-architecture/archives.html)
