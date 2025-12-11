# Moving and Copying Items

This page covers how to move and copy Sitecore items using SPE, including transfers within databases and between databases.

## Moving Items

Use `Move-Item` to transfer items from one location to another. The item retains its ID and all version history.

### Basic Move Operations

**Example:** Move an item to a new parent.

```powershell
$sourcePath = "master:\content\home\sample item"
$destinationPath = "master:\content\home\moved"
Move-Item -Path $sourcePath -Destination $destinationPath
```

{% hint style="info" %}
If the destination item exists, the moved item becomes a child of the destination. If the destination doesn't exist, the source item is renamed during the move.
{% endhint %}

**Example:** Move and rename in one operation.

```powershell
# If "new-name" doesn't exist, item is moved and renamed
$oldPath = "master:\content\home\old-name"
$newPath = "master:\content\home\new-name"
Move-Item -Path $oldPath -Destination $newPath
```

### Moving via Pipeline

**Example:** Get an item and move it using the pipeline.

```powershell
Get-Item -Path "master:" -ID "{65736CA0-7D69-452A-A16F-2F42264D21C5}" |
    Move-Item -Destination "master:{DFDDF372-3AB7-45B1-9E7C-0D0B27350439}"
```

### Moving with Children

By default, `Move-Item` moves the item and all its descendants.

**Example:** Move an entire content tree.

```powershell
Move-Item -Path "master:\content\home\old-section" `
          -Destination "master:\content\archive\"
```

## Copying Items

Use `Copy-Item` to duplicate items. By default, the copy receives a new ID.

### Basic Copy Operations

**Example:** Copy an item to a new location and change display name.

```powershell
$originalPath = "master:\content\home\original"
$copyPath = "master:\content\home\copy"
Copy-Item -Path $originalPath -Destination $copyPath
```

{% hint style="info" %}
The item name in the destination path determines the new item's name. Lowercase names in the destination result in lowercase item names.
{% endhint %}

**Example:** Copy and return the new item.

```powershell
$originalPath = "master:\content\home\original"
$copyPath = "master:\content\home\copy"
$newItem = Copy-Item -Path $originalPath -Destination $copyPath -PassThru
$newItem."__Display Name" = "Copy"

Write-Host "New item ID: $($newItem.ID)"
```

### Recursive Copying

**Example:** Copy an entire tree maintaining structure.

```powershell
$sourceId = "{AF27FAD3-2AF0-4682-9BF7-375197587579}"
$destinationId = "{53F94442-555B-4622-B813-A16ED2CAB01B}"

Get-Item -Path "master:" -ID $sourceId | Copy-Item -Destination $destinationId -Recurse
```

### Copying Between Databases

Copy items between databases (e.g., from master to web) using transfer options.

**Example:** Transfer item with same ID to another database.

```powershell
Copy-Item -Path "master:\content\home" `
          -Destination "web:\content\home" `
          -TransferOptions 0
```

{% hint style="warning" %}
Using `-TransferOptions 0` maintains the same ID. This is useful for publishing workflows but can cause conflicts if the item already exists in the target database.
{% endhint %}

### Transfer Options

The `-TransferOptions` parameter controls how items are copied between databases:

| Value | Behavior                         |
| ----- | -------------------------------- |
| 0     | Keep original ID                 |
| 1     | Create new ID (default behavior) |
| 2     | Allow default values             |
| 4     | Allow standard values            |

**Example:** Copy with new ID (default behavior).

```powershell
# These are equivalent
Copy-Item -Path "master:\content\home\sampleitem" -Destination "web:\content\home\sampleitem"
Copy-Item -Path "master:\content\home\sampleitem" -Destination "web:\content\home\sampleitem" -TransferOptions 1
```

## Bulk Operations

### Pattern: Move Multiple Items

```powershell
Get-ChildItem -Path "master:\content\temp" -Recurse |
    Where-Object { $_.TemplateName -eq "Sample Item" } |
    ForEach-Object {
        Move-Item -Path $_.ProviderPath -Destination "master:\content\archive"
    }
```

### Pattern: Copy Items Based on Criteria

```powershell
$templates = @("Sample Item", "Article", "News Item")

Get-ChildItem -Path "master:\content\home" -Recurse |
    Where-Object { $templates -contains $_.TemplateName } |
    ForEach-Object {
        Copy-Item -Path $_.ProviderPath -Destination "master:\content\backup"
    }
```

### Pattern: Reorganize Content Structure

```powershell
# Move all items of a specific template to a new location
$targetTemplate = "Article"
$newParent = Get-Item -Path "master:\content\articles"

Get-ChildItem -Path "master:\content\home" -Recurse |
    Where-Object { $_.TemplateName -eq $targetTemplate } |
    ForEach-Object {
        Move-Item -Path $_.ProviderPath -Destination $newParent.ProviderPath
        Write-Host "Moved: $($_.Name)"
    }
```

### Pattern: Copy with Progress Reporting

```powershell
$itemsToCopy = Get-ChildItem -Path "master:\content\home\original" -Recurse
$total = $itemsToCopy.Count
$current = 0

foreach($item in $itemsToCopy) {
    $current++
    Write-Progress -Activity "Copying items" `
                   -Status "Copying $($item.Name)" `
                   -PercentComplete (($current / $total) * 100)

    Copy-Item -Path $item.ProviderPath `
              -Destination "master:\content\home\copied" `
              -Recurse
}
Write-Progress -Activity "Copying items" -Completed
```

## Maintaining Structure

### Pattern: Copy Tree Maintaining Hierarchy

```powershell
$sourceRoot = Get-Item -Path "master:\content\home\job-search"
$destinationRoot = Get-Item -Path "master:\content\home\careers"

$sourceRoot | Copy-Item -Destination $destinationRoot.ProviderPath -Recurse

# Produces a new tree /sitecore/content/home/careers/job-search
# Existing content remains at /sitecore/content/home/job-search
```

## Dynamic Parameters

| Parameter       | Command              | Description                              | Example                    |
| --------------- | -------------------- | ---------------------------------------- | -------------------------- |
| DestinationItem | Move-Item, Copy-Item | Parent item to receive moved/copied item | `-DestinationItem $parent` |
| FailSilently    | Move-Item, Copy-Item | Suppress unauthorized access errors      | `-FailSilently`            |
| TransferOptions | Copy-Item            | Controls ID handling (0=keep, 1=new)     | `-TransferOptions 0`       |
| PassThru        | Copy-Item            | Returns the new item                     | `-PassThru`                |
| Recurse         | Copy-Item            | Includes all descendants                 | `-Recurse`                 |

## Performance Considerations

- **Batch operations** - Group multiple moves/copies together
- **Use BulkUpdateContext** - When copying many items with field updates
- **Progress reporting** - Provide feedback for long-running operations
- **Avoid unnecessary recursion** - Only use `-Recurse` when needed
- **Test first** - Use `-WhatIf` parameter if available, or test in development

## Common Pitfalls

### Pitfall: Moving to Non-Existent Path

```powershell
# BAD - if parent doesn't exist, item is renamed
Move-Item -Path "master:\content\home\demo" `
          -Destination "master:\content\nonexistent"
```

```powershell
# GOOD - verify parent exists
$destination = "master:\content\archive"
if (Test-Path $destination) {
    Move-Item -Path "master:\content\home\demo" -Destination $destination
} else {
    Write-Host "Destination does not exist" -ForegroundColor Red
}
```

### Pitfall: Copying Without PassThru

```powershell
# BAD - can't access the new item
Copy-Item -Path "master:\content\home\demo" -Destination "master:\content\copy"
# No reference to the copied item
```

```powershell
# GOOD - use PassThru to get reference
$sourcePath = "master:\content\home\demo"
$destinationPath = "master:\content\home\demo-copy"
$copiedItem = Copy-Item -Path $sourcePath -Destination $destinationPath -PassThru
$copiedItem.Title = "Updated Title"
```

## See Also

- [Retrieving Items](retrieving-items.md) - Find items to move or copy
- [Creating and Removing Items](creating-and-removing-items.md) - Item lifecycle management
- [Best Practices](best-practices.md) - Performance optimization
- [Appendix - Common Commands](../appendix/common/README.md) - Full cmdlet reference

## References

- [Working with Sitecore Items in PowerShell Extensions](https://blog.najmanowicz.com/2014/10/12/working-with-sitecore-items-in-powershell-extensions/)
