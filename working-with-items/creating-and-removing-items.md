# Creating and Removing Items

This page covers how to create new items and remove existing items using SPE.

## Creating Items

Use `New-Item` to create new Sitecore items. The cmdlet requires a path or parent item, a name, and a template reference.

### Basic Item Creation

**Example:** Create an item by specifying template name.

```powershell
$itemPath = "master:\content\home\Sample Item 3"
New-Item -Path $itemPath -ItemType "Sample/Sample Item"
```

**Output:**

```powershell

Name                             Children Language Version Id                                     TemplateName
----                             -------- -------- ------- --                                     ------------
Sample Item 3                    False    en       1       {29A68114-5137-4A46-A87C-3789B8D898FB} Sample Item
```

{% hint style="info" %}
The `-ItemType` parameter accepts either the template path (e.g., "Sample/Sample Item") or the template ID.
{% endhint %}

**Example:** Create an item using template ID.

```powershell
$itemPath = "master:\content\home\Sample Item 4"
$templateId = "{76036F5E-CBCE-46D1-AF0A-4143F9B557AA}"
New-Item -Path $itemPath -ItemType $templateId
```

### Creating Items with Specific IDs

By default, Sitecore generates a new GUID for each item. Use `-ForceId` to specify a custom ID.

**Example:** Create an item with a predefined ID.

```powershell
$itemPath = "master:\content\home\Sample Item 5"
$templateId = "{76036F5E-CBCE-46D1-AF0A-4143F9B557AA}"
$itemId = "{9459ADDD-4471-4ED3-A041-D33E559BD321}"
New-Item -Path $itemPath -ItemType $templateId -ForceId $itemId
```

**Output:**

```powershell

Name                             Children Language Version Id                                     TemplateName
----                             -------- -------- ------- --                                     ------------
Sample Item 5                    False    en       1       {9459ADDD-4471-4ED3-A041-D33E559BD321} Sample Item
```

{% hint style="warning" %}
Using `-ForceId` with an existing ID will cause an error. Only use this when migrating content or when you need a specific ID for integration purposes.
{% endhint %}

### Creating Items as Children

Instead of specifying the full path, you can pass a parent item using the `-Parent` parameter.

**Example:** Create a child item under a parent.

```powershell
$templateId = "{76036F5E-CBCE-46D1-AF0A-4143F9B557AA}"
$parentItem = Get-Item -Path "master:" -ID "{110D559F-DEA5-42EA-9C1C-8A5DF7E70EF9}"
New-Item -Parent $parentItem -Name "Sample Item 6" -ItemType $templateId
```

**Output:**

```powershell

Name                             Children Language Version Id                                     TemplateName
----                             -------- -------- ------- --                                     ------------
Sample Item 6                    False    en       1       {E7A73426-B9BB-4056-A15C-2E835796A4DD} Sample Item
```

### Creating Items in Specific Languages

Use the `-Language` parameter to create items in specific language versions.

**Example:** Create an item with a specific language version.

```powershell
$itemPath = "master:\content\home\Sample Item 7"
New-Item -Path $itemPath -ItemType "Sample/Sample Item" -Language "en-CA"
```

**Output:**

```powershell

Name                             Children Language Version Id                                     TemplateName
----                             -------- -------- ------- --                                     ------------
Sample Item 7                    False    en-CA    1       {5303A55C-5853-409D-AFC8-0DF04F4C1065} Sample Item
```

{% hint style="info" %}
When you specify a language during creation, only that language version is created. Other language versions are not automatically added.
{% endhint %}

### Starting Workflows

Use the `-StartWorkflow` parameter to initiate the default workflow after item creation.

**Example:** Create an item and start its workflow.

```powershell
$itemPath = "master:\content\home\Sample Item 8"
New-Item -Path $itemPath -ItemType "Sample/Sample Item" -StartWorkflow
```

### Setting Field Values During Creation

After creating an item, you can immediately set field values.

**Example:** Create and populate an item.

```powershell
$item = New-Item -Path "master:\content\home\demo" -Name "New Page" -ItemType "Sample/Sample Item"
$item.Title = "Welcome to Our Site"
$item.Text = "This is the page content"
$item."Meta Description" = "SEO description for this page"
```

## Bulk Item Creation

### Pattern: Create Items from CSV

```powershell
# Assumes you uploaded a file to a folder on the server hosting Sitecore
$data = Import-Csv "$($SitecoreDataFolder)\import\pages.csv"
$parentPath = "master:\content\home"
$templateId = "{76036F5E-CBCE-46D1-AF0A-4143F9B557AA}"

foreach($row in $data) {
    $item = New-Item -Parent (Get-Item $parentPath) -Name $row.Name -ItemType $templateId
    $item.Title = $row.Title
    $item.Text = $row.Text
}
```

### Pattern: Create Items with Progress Reporting

```powershell
$itemsToCreate = @("Page 1", "Page 2", "Page 3", "Page 4", "Page 5")
$total = $itemsToCreate.Count
$current = 0

foreach($name in $itemsToCreate) {
    $current++
    Write-Progress -Activity "Creating items" -Status "Creating $name" `
        -PercentComplete (($current / $total) * 100)

    New-Item -Path "master:\content\home" -Name $name -ItemType "Sample/Sample Item"
}
Write-Progress -Activity "Creating items" -Completed
```

### Pattern: Create Hierarchical Structure

```powershell
$structure = @{
    "Products" = @("Category A", "Category B", "Category C")
    "Services" = @("Consulting", "Training", "Support")
    "About" = @("Team", "History", "Contact")
}

foreach($section in $structure.Keys) {
    $sectionItem = New-Item -Path "master:\content\home" -Name $section -ItemType "Common/Folder"

    foreach($page in $structure[$section]) {
        New-Item -Parent $sectionItem -Name $page -ItemType "Sample/Sample Item"
    }
}
```

### Pattern: Create Items from JSON

```powershell
$json = Get-Content "$($SitecoreDataFolder)\import\structure.json" | ConvertFrom-Json

foreach($item in $json.items) {
    $newItem = New-Item -Path "master:\content\home" -Name $item.name -ItemType $item.template
    $newItem.Title = $item.title
    $newItem.Text = $item.text
}
```

## Removing Items

Use `Remove-Item` to delete items. By default, items are moved to the Recycle Bin unless you specify `-Permanently`.

### Basic Item Removal

**Example:** Remove an item (sends to Recycle Bin).

```powershell
Remove-Item -Path "master:\content\home\Sample Item 3"
```

**Example:** Remove an item permanently (bypasses Recycle Bin).

```powershell
Remove-Item -Path "master:\content\home\Sample Item 3" -Permanently
```

{% hint style="danger" %}
Using `-Permanently` cannot be undone. Items are deleted from the database immediately. Use with extreme caution, especially in production environments.
{% endhint %}

### Removing Items via Pipeline

**Example:** Remove items from pipeline.

```powershell
Get-Item -Path "master:\content\home\delete-me" | Remove-Item
```

**Example:** Remove multiple items matching criteria.

```powershell
Get-ChildItem -Path "master:\content\temp" -Recurse |
    Where-Object { $_.Name -like "Test*" } |
    Remove-Item
```

### Bulk Removal Patterns

### Pattern: Remove Old Items

```powershell
$cutoffDate = (Get-Date).AddYears(-2)
Get-ChildItem -Path "master:\content\archive" -Recurse |
    Where-Object { $_.__Created -lt $cutoffDate } |
    Remove-Item -Permanently
```

### Pattern: Remove Items by Template

```powershell
Get-ChildItem -Path "master:\content\home" -Recurse |
    Where-Object { $_.TemplateName -eq "Temporary Item" } |
    Remove-Item
```

### Pattern: Clean Up Empty Folders

```powershell
Get-ChildItem -Path "master:\content\home" -Recurse |
    Where-Object { $_.TemplateName -eq "Folder" -and -not $_.HasChildren } |
    Remove-Item
```

### Pattern: Remove with Confirmation

```powershell
$itemsToRemove = Get-ChildItem -Path "master:\content\home\temp" -Recurse

foreach($item in $itemsToRemove) {
    $response = Show-Confirm -Title "Remove $($item.ItemPath)?"
    if ($response -eq 'yes') {
        Remove-Item -Path $item.ProviderPath
        Write-Host "Removed: $($item.ItemPath)" -ForegroundColor Green
    }
}
```

### Pattern: Safe Removal with Reporting

```powershell
$itemsToRemove = Get-ChildItem -Path "master:\content\temp" -Recurse
$report = @()

foreach($item in $itemsToRemove) {
    try {
        Remove-Item -Path $item.ProviderPath -Permanently
        $report += [PSCustomObject]@{
            ItemPath = $item.ItemPath
            Status = "Deleted"
            Error = ""
        }
    } catch {
        $report += [PSCustomObject]@{
            ItemPath = $item.ItemPath
            Status = "Failed"
            Error = $_.Exception.Message
        }
    }
}

New-Item -Path "$($SitecoreDataFolder)\export" -ItemType Directory -Force
$report | Export-Csv "$($SitecoreDataFolder)\export\deletion-report.csv" -NoTypeInformation
```

## Dynamic Parameters

Both `New-Item` and `Remove-Item` support dynamic parameters when working with Sitecore paths:

| Parameter     | Command     | Description                       | Example               |
| ------------- | ----------- | --------------------------------- | --------------------- |
| ForceId       | New-Item    | Forces specific GUID for new item | `-ForceId "{GUID}"`   |
| Language      | New-Item    | Creates item in specific language | `-Language "en-CA"`   |
| Parent        | New-Item    | Specifies parent item             | `-Parent $parentItem` |
| StartWorkflow | New-Item    | Initiates default workflow        | `-StartWorkflow`      |
| Permanently   | Remove-Item | Bypasses Recycle Bin              | `-Permanently`        |
| FailSilently  | Remove-Item | Suppresses errors                 | `-FailSilently`       |

## Item Naming Considerations

When creating items, Sitecore applies item naming rules:

### Valid Item Names

- Alphanumeric characters (a-z, A-Z, 0-9)
- Hyphens and underscores (-, \_)
- Spaces (converted to hyphens in URLs)

### Invalid Characters

Sitecore automatically filters or replaces these characters:

- Forward slash (/)
- Backslash (\)
- Question mark (?)
- Hash (#)
- Colon (:)
- Semicolon (;)
- Brackets ([, ])

**Example:** Create items with special character handling.

```powershell
$name = [Sitecore.Data.Items.ItemUtil]::ProposeValidItemName('Page: About Us')

# These names will be automatically cleaned
New-Item -Path "master:\content\home" -Name $name -ItemType "Sample/Sample Item"
# Results in item name: "Page About Us" or similar
```

```powershell
$name = [Sitecore.Data.Items.ItemUtil]::ProposeValidItemName('Page: About Us')
# Use Display Name for formatted names
$item = New-Item -Path "master:\content\home" -Name "page-about-us" -ItemType "Sample/Sample Item"
$item."__Display name" = $name
```

## Error Handling

Always handle potential errors when creating or removing items.

### Pattern: Create with Error Handling

```powershell
try {
    $item = New-Item -Path "master:\content\home\new-page" -Name "New Page" -ItemType "Sample/Sample Item" -ErrorAction Stop
    Write-Host "Created: $($item.ItemPath)" -ForegroundColor Green
} catch {
    Write-Host "Error creating item: $($_.Exception.Message)" -ForegroundColor Red -BackgroundColor White
}
```

### Pattern: Bulk Create with Error Logging

```powershell
$results = @()

foreach($name in $itemNames) {
    try {
        $item = New-Item -Path "master:\content\home" -Name $name -ItemType "Sample/Sample Item" -ErrorAction Stop
        $results += [PSCustomObject]@{
            Name = $name
            Status = "Success"
            ItemId = $item.ID
            Error = ""
        }
    } catch {
        $results += [PSCustomObject]@{
            Name = $name
            Status = "Failed"
            ItemId = ""
            Error = $_.Exception.Message
        }
    }
}

New-Item -Path "$($SitecoreDataFolder)\export" -ItemType Directory -Force
$results | Export-Csv "$($SitecoreDataFolder)\export\creation-results.csv" -NoTypeInformation
```

## Performance Tips

- **Batch operations** - Create or remove many items in a single script execution
- **Use BulkUpdateContext** - When creating many items with field values
- **Use SecurityDisabler** - If creating items programmatically (no user context needed)
- **Disable events** - For large migrations where events aren't needed
- **Progress reporting** - Always provide feedback for long-running operations

**Example:** Optimized bulk creation.

```powershell
$largeDataset = @(
    [PSCustomObject]@{"Name"="BulkItem1";"Template"="Sample/Sample Item";"Title"="BulkItem1 Title";"Text"="BulkItem1 Text";},
    [PSCustomObject]@{"Name"="BulkItem2";"Template"="Sample/Sample Item";"Title"="BulkItem2 Title";"Text"="BulkItem2 Text";},
    [PSCustomObject]@{"Name"="BulkItem3";"Template"="Sample/Sample Item";"Title"="BulkItem3 Title";"Text"="BulkItem3 Text";}
)

New-UsingBlock (New-Object Sitecore.Data.BulkUpdateContext) {
    New-UsingBlock (New-Object Sitecore.Data.Events.EventDisabler) {
        foreach($data in $largeDataset) {
            $item = New-Item -Path "master:\content\home" -Name $data.Name -ItemType $data.Template
            $item.Editing.BeginEdit()
            $item["Title"] = $data.Title
            $item["Text"] = $data.Text
            $item.Editing.EndEdit() > $null
        }
    }
}

# Clear all the caches may be required if working in the Content Editor
# [Sitecore.Caching.CacheManager]::ClearAllCaches()
```

## See Also

- [Retrieving Items](retrieving-items.md) - Find items to work with
- [Editing Items](editing-items.md) - Update item properties
- [Moving and Copying Items](moving-and-copying-items.md) - Transfer items
- [Best Practices](best-practices.md) - Performance optimization
- [Appendix - Common Commands](../appendix/common/README.md) - Full cmdlet reference

## References

- [Working with Sitecore Items in PowerShell Extensions](https://blog.najmanowicz.com/2014/10/12/working-with-sitecore-items-in-powershell-extensions/)
