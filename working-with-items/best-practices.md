# Best Practices

This page provides guidance on writing efficient, maintainable, and performant SPE scripts for working with Sitecore items.

## Performance Optimization

### Query Method Selection

Choose the right query method based on your use case:

| Method             | Best For                        | Performance | Security | Full Item Data |
| ------------------ | ------------------------------- | ----------- | -------- | -------------- |
| `Get-Item` by path | Single known item               | ⭐⭐⭐      | ✓        | ✓              |
| `Get-ChildItem`    | Small trees (<500 items)        | ⭐⭐        | ✓        | ✓              |
| Sitecore Query     | Specific criteria, medium trees | ⭐⭐        | ✓        | ✓              |
| `Find-Item`        | Large trees (>500 items)        | ⭐⭐⭐⭐    | ✓        | Partial        |

**Example:** Performance comparison for finding items by template.

```powershell
# SLOW - Retrieves all items then filters
$items = Get-ChildItem -Path "master:\content" -Recurse |
    Where-Object { $_.TemplateName -eq "Sample Item" }

# FASTER - Uses Sitecore query to filter server-side
$items = Get-Item -Path "master:" -Query "/sitecore/content//*[@@templatename='Sample Item']"

# FASTEST - Uses the search index
# See Find-Item
```

### Bulk Update Performance

When updating many items, use appropriate context objects to improve performance:

**Example:** Performance tiers for bulk updates.

```powershell
# SLOWEST - Automated properties call BeginEdit/EndEdit each time
foreach($item in $items) {
    $item.Title = "Title"        # BeginEdit/EndEdit
    $item.Text = "Text"          # BeginEdit/EndEdit
    $item.Date = [datetime]::Now # BeginEdit/EndEdit
}

# BETTER - Manual BeginEdit/EndEdit once per item
foreach($item in $items) {
    $item.Editing.BeginEdit()
    $item["Title"] = "Title"
    $item["Text"] = "Text"
    $item["Date"] = [Sitecore.DateUtil]::ToIsoDate([datetime]::Now)
    $item.Editing.EndEdit() > $null
}

# BEST - BulkUpdateContext for large datasets
New-UsingBlock (New-Object Sitecore.Data.BulkUpdateContext) {
    foreach($item in $items) {
        $item.Editing.BeginEdit()
        $item["Title"] = "Title"
        $item["Text"] = "Text"
        $item.Editing.EndEdit() > $null
    }
}

# OPTIMAL - Combine with EventDisabler when events aren't needed
New-UsingBlock (New-Object Sitecore.Data.BulkUpdateContext) {
    New-UsingBlock (New-Object Sitecore.Data.Events.EventDisabler) {
        foreach($item in $items) {
            $item.Editing.BeginEdit()
            $item["Title"] = "Title"
            $item["Text"] = "Text"
            $item.Editing.EndEdit() > $null
        }
    }
}
```

### Memory Management

For large datasets, process items in batches to avoid memory issues. Use `Find-Item` with pagination to retrieve items server-side rather than loading everything into memory:

**Example:** Efficient batch processing with search index pagination.

```powershell
$batchSize = 250
$offset = 0
$totalProcessed = 0

# Define search criteria
$criteria = @(
    @{Filter = "Equals"; Field = "_templatename"; Value = "Sample Item"},
    @{Filter = "DescendantOf"; Value = (Get-Item "master:\content").ID}
)

$props = @{
    Index = "sitecore_master_index"
    Criteria = $criteria
}

do {
    # Query batch from search index (server-side pagination)
    $searchItems = @(Find-Item @props -Skip $offset -First $batchSize)

    if ($searchItems.Count -eq 0) { break }

    # Process batch with bulk update context
    New-UsingBlock (New-Object Sitecore.Data.BulkUpdateContext) {
        foreach($searchItem in $searchItems) {
            # Convert search result to full Sitecore item
            # Pipe items to Initialize-Item or call Get-Item with the ID

            # The prefix `$script is required because of the ScriptBlock closure created by New-UsingBlock
            $script:totalProcessed++
        }
    }

    $offset += $searchItems.Count
    Write-Host "Processed: $totalProcessed items"
} while ($searchItems.Count -eq $batchSize)

Write-Host "Completed processing $totalProcessed items"

# Processed: 1 items
# Completed processing 1 items
```

### Caching Considerations

//TODO

## Security Best Practices

//TODO

### Validating Access

//TODO

## Code Organization

### Function Structure

Create reusable functions with proper error handling:

**Example:** Well-structured function.

```powershell
function Update-ItemTitle {
    [CmdletBinding(SupportsShouldProcess)]
    param(
        [Parameter(Mandatory, ValueFromPipeline)]
        [Sitecore.Data.Items.Item]$Item,

        [Parameter(Mandatory)]
        [string]$Title
    )

    process {
        try {
            # Validation
            if (-not $Item) {
                Write-Error "Item is null"
                return
            }

            # WhatIf support
            if ($PSCmdlet.ShouldProcess($Item.ItemPath, "Update title to '$Title'")) {
                $Item.Editing.BeginEdit()
                try {
                    $Item["Title"] = $Title
                    $Item.Editing.EndEdit()
                    Write-Verbose "Updated: $($Item.ItemPath)"
                } catch {
                    $Item.Editing.CancelEdit()
                    throw
                }
            }
        } catch {
            Write-Error "Failed to update $($Item.ItemPath): $($_.Exception.Message)"
        }
    }
}

# Usage
Get-ChildItem -Path "master:\content\" |
    Update-ItemTitle -Title "New Title" -Verbose -WhatIf
```

### Error Handling

Implement comprehensive error handling:

**Example:** Robust error handling pattern.

```powershell
$itemPaths = @(
    "master:/content/Home"
)

$results = @()
$errors = @()

foreach($itemPath in $itemPaths) {
    try {
        $item = Get-Item -Path $itemPath -ErrorAction Stop

        $item.Editing.BeginEdit()
        try {
            $item["Title"] = "Updated"
            $item.Editing.EndEdit()

            $results += [PSCustomObject]@{
                ItemPath = $item.ItemPath
                Status = "Success"
                Error = ""
            }
        } catch {
            $item.Editing.CancelEdit()
            throw
        }
    } catch {
        $errors += [PSCustomObject]@{
            ItemPath = $itemPath
            Status = "Failed"
            Error = $_.Exception.Message
        }
    }
}

# Report results
$results | Show-ListView
```

## Progress Reporting

Always provide feedback for long-running operations:

**Example:** Comprehensive progress reporting.

```powershell
function Update-ItemsWithProgress {
    param(
        [Sitecore.Data.Items.Item[]]$Items,
        [scriptblock]$UpdateAction
    )

    $total = $Items.Count
    $current = 0
    $sw = [System.Diagnostics.Stopwatch]::StartNew()

    foreach($item in $Items) {
        $current++

        # Calculate ETA
        $percentComplete = ($current / $total) * 100
        $elapsedSeconds = $sw.Elapsed.TotalSeconds
        $estimatedTotalSeconds = ($elapsedSeconds / $current) * $total
        $remainingSeconds = $estimatedTotalSeconds - $elapsedSeconds

        Write-Progress -Activity "Processing items" `
                       -Status "Processing $($item.Name) ($current of $total)" `
                       -PercentComplete $percentComplete `
                       -SecondsRemaining $remainingSeconds

        & $UpdateAction $item
    }

    $sw.Stop()
    Write-Progress -Activity "Processing items" -Completed
    Write-Host "Completed $total items in $($sw.Elapsed.TotalSeconds.ToString('F2')) seconds"
}

# Usage
$items = Get-ChildItem -Path "master:\content\" -Recurse
Update-ItemsWithProgress -Items $items -UpdateAction {
    param($item)
    $item.Title = "Updated: $($item.Name)"
}
```

**Note:** Use the Bulk Data Generator found under the Toolbox to create test items.

## Language and Version Handling

### Working with Multiple Languages

Be explicit about language handling:

**Example:** Multi-language update pattern.

```powershell
$languages = @("en", "de-DE", "da", "es-ES")

foreach($lang in $languages) {
    $item = Get-Item -Path "master:\content\home" -Language $lang -ErrorAction SilentlyContinue

    if ($item) {
        $item.Title = "Localized Title for $lang"
        Write-Host "Updated $lang version"
    } else {
        Write-Warning "No $lang version exists"
    }
}
```

### Version Management

Handle versions carefully to avoid unintended consequences:

**Example:** Safe version operations.

```powershell
# Get latest version
$item = Get-Item -Path "master:\content\home" -Language "en"

# Create new version before updating
$newVersion = $item.Versions.AddVersion()
$newVersion.Editing.BeginEdit()
$newVersion["Title"] = "Updated in new version"
$newVersion.Editing.EndEdit()

Write-Host "Created version $($newVersion.Version)"
```

## Field Type Handling

### Type-Safe Field Access

Use typed field access when working with complex fields:

**Example:** Safe field type handling.

```powershell
$item = Get-Item -Path "master:\content\home"

# Use the Automatic Properties
Write-Host "Title: $($item.Title)"

# Use Typed Fields
$textField = $item.PSFields.Text
if($textField.Value) {
    Write-Host "Text: $($item.PSFields.Text.Value)"
}

# Use typed fields for complex types
$linkField = [Sitecore.Data.Fields.LinkField]$item.Fields["Link"]
if ($linkField -and $linkField.LinkType -eq "internal") {
    $targetItem = $linkField.TargetItem
    Write-Host "Links to: $($targetItem.ItemPath)"
}

# Use typed fields for images
$imageField = [Sitecore.Data.Fields.ImageField]$item.Fields["Image"]
if ($imageField -and $imageField.MediaItem) {
    Write-Host "Image: $($imageField.MediaItem.Name)"
    Write-Host "Alt: $($imageField.Alt)"
    Write-Host "Size: $($imageField.Width)x$($imageField.Height)"
}
```

### Date Handling

Always use ISO format for date fields:

**Example:** Proper date handling.

```powershell
$item = Get-Item -Path "master:\content\home"

# CORRECT - Use ISO format for manual field assignment
$item.Editing.BeginEdit()
$item["MyDateField"] = [Sitecore.DateUtil]::ToIsoDate([datetime]::Now)
$item.Editing.EndEdit()

# ALSO CORRECT - Automatic properties handle conversion
$item.MyDateField = [datetime]::Now

# Reading dates
$createdDate = [Sitecore.DateUtil]::IsoDateToDateTime($item["__Created"])
# Or using automated properties
$createdDate = $item.__Created  # Returns System.DateTime
```

## Testing and Validation

### Dry Run Support

Implement dry-run mode for destructive operations:

**Example:** Dry-run pattern.

```powershell
function Remove-TestContent {
    param(
        [switch]$WhatIf
    )
    
    $itemsToDelete = Get-ChildItem -Path "master:\content\" -Recurse
    
    foreach($item in $itemsToDelete) {
        if ($WhatIf) {
            Write-Host "WHATIF: Would delete $($item.ItemPath)" -ForegroundColor Yellow
        } else {
            #Remove-Item -Path $item.ProviderPath -Permanently
            Write-Host "Deleted: $($item.ItemPath)" -ForegroundColor Red -BackgroundColor White
        }
    }
}
Remove-TestContent -WhatIf
```

### Validation Before Operations

Validate data before making changes:

//TODO

## Logging and Auditing

### Comprehensive Logging

Log operations for audit trails:

**Example:** Logging pattern.

```powershell
Write-Log "Starting item updates"
$items = Get-ChildItem -Path "master:\content"
foreach($item in $items) {
    try {
        $item.Title = "Updated"
        Write-Log "Updated: $($item.ItemPath)"
    } catch {
        Write-Log "Failed: $($item.ItemPath) - $($_.Exception.Message)" -Log Error
    }
}

Write-Log "Completed item updates"
```

## Common Anti-Patterns

### ❌ Anti-Pattern: Not Checking Item Existence

```powershell
# BAD
$item = Get-Item -Path "master:\content\maybe-exists"
$item.Title = "New Title"  # Throws if item doesn't exist
```

```powershell
# GOOD
$item = Get-Item -Path "master:\content\maybe-exists" -ErrorAction SilentlyContinue
if ($item) {
    $item.Title = "New Title"
} else {
    Write-Warning "Item not found"
}
```

### ❌ Anti-Pattern: Inefficient Queries

//TODO

### ❌ Anti-Pattern: Swallowing Errors

```powershell
# BAD
foreach($item in $items) {
    try {
        $item.Title = "Updated"
    } catch {
        # Silent failure
    }
}
```

```powershell
# GOOD
$items = Get-ChildItem -Path "master:\content"

$errors = @()
foreach($item in $items) {
    try {
        $item.Title = "Updated"
    } catch {
        $errors += [PSCustomObject]@{
            Item = $item.ItemPath
            Error = $_.Exception.Message
        }
    }
}

if ($errors.Count -gt 0) {
    $errors | Show-ListView
}
```

## Performance Benchmarking

Use `Measure-Command` to identify bottlenecks:

**Example:** Performance comparison.

```powershell
# Method 1
$time1 = Measure-Command {
    $items = Get-ChildItem -Path "master:\content" -Recurse |
        Where-Object { $_.TemplateName -eq "Sample Item" }
}

# Method 2
$time2 = Measure-Command {
    $items = Get-Item -Path "master:" -Query "/sitecore/content//*[@@templatename='Sample Item']"
}

Write-Host "Method 1: $($time1.TotalSeconds)s"
Write-Host "Method 2: $($time2.TotalSeconds)s"
Write-Host "Improvement: $(($time1.TotalSeconds / $time2.TotalSeconds).ToString('P0'))"
```

## See Also

- [Retrieving Items](retrieving-items.md) - Query optimization techniques
- [Editing Items](editing-items.md) - Update performance patterns
- [Item Security](item-security.md) - Security best practices
- [Appendix - Common Commands](../appendix/common/README.md) - Cmdlet reference

## References

- [Sitecore Query Performance](https://sitecore.stackexchange.com/questions/6803/sitecore-powershell-query-for-big-images-of-a-certain-size/6811#6811)
- [PowerShell Best Practices](https://docs.microsoft.com/en-us/powershell/scripting/developer/cmdlet/cmdlet-development-guidelines)
