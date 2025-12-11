# Working with Items

The Sitecore PowerShell Extensions (SPE) module provides powerful cmdlets for managing Sitecore content items. This section covers everything you need to know about working with items programmatically.

## Quick Start

The core item management cmdlets mirror standard PowerShell conventions:

| Cmdlet | Purpose | Example |
|--------|---------|---------|
| `Get-Item` | Retrieve a single item | `Get-Item -Path "master:\content\home"` |
| `Get-ChildItem` | Retrieve children and descendants | `Get-ChildItem -Path "master:\content" -Recurse` |
| `New-Item` | Create a new item | `New-Item -Path "master:\content\home" -Name "Page" -ItemType "Sample/Sample Item"` |
| `Set-ItemProperty` | Update item properties | `Set-ItemProperty -Path "master:\content\home" -Name "Title" -Value "Welcome"` |
| `Copy-Item` | Duplicate an item | `Copy-Item -Path "master:\content\home\source" -Destination "master:\content\home\target"` |
| `Move-Item` | Transfer an item | `Move-Item -Path "master:\content\home\old" -Destination "master:\content\home\new"` |
| `Remove-Item` | Delete or recycle an item | `Remove-Item -Path "master:\content\home\demo" -Permanently` |

## Documentation Topics

### Fundamentals

Before working with items, understand these foundational concepts:

- **[Providers](providers.md)** - PowerShell providers and navigating Sitecore databases
- **[Variables](variables.md)** - Built-in variables available in SPE scripts

### Core Item Operations

Learn how to perform essential item operations:

- **[Retrieving Items](retrieving-items.md)** - Find items by path, ID, query, or URI
- **[Editing Items](editing-items.md)** - Update properties, work with field types, and manage versions
- **[Creating and Removing Items](creating-and-removing-items.md)** - Create new items and delete existing ones
- **[Moving and Copying Items](moving-and-copying-items.md)** - Transfer and duplicate items

### Specialized Operations

Handle specific aspects of item management:

- **[Item Languages](item-languages.md)** - Add, remove, and manage language versions
- **[Item Renderings](item-renderings.md)** - Work with presentation details and renderings
- **[Item Security](item-security.md)** - Manage item permissions and access control

### Advanced Topics

Optimize your scripts and follow best practices:

- **[Best Practices](best-practices.md)** - Performance optimization, bulk operations, and coding patterns

## Common Patterns

### Pattern: Find and Update Items

```powershell
# Find all items of a specific template and update a field
Get-ChildItem -Path "master:\content\home" -Recurse |
    Where-Object { $_.TemplateName -eq "Sample Item" } |
    ForEach-Object {
        $_.Title = "Updated Title"
    }
```

### Pattern: Bulk Create Items

```powershell
# Create multiple items from a data source
$data = Import-Csv "C:\data\pages.csv"
foreach($row in $data) {
    New-Item -Path "master:\content\home" -Name $row.Name -ItemType "Sample/Sample Item"
}
```

### Pattern: Copy Content Between Databases

```powershell
# Copy an item tree from master to web
$sourceItem = Get-Item -Path "master:\content\home\campaign"
Copy-Item -Path $sourceItem.ProviderPath -Destination "web:\content\home" -Recurse
```

### Pattern: Audit and Report

```powershell
# Generate a report of items modified in the last 7 days
$startDate = (Get-Date).AddDays(-7)
Get-ChildItem -Path "master:\content" -Recurse |
    Where-Object { $_.__Updated -gt $startDate } |
    Select-Object Name, ItemPath, __Updated, "__Updated by" |
    Show-ListView -Property Name, ItemPath, __Updated, "__Updated by"
```

## Dynamic Parameters

SPE extends standard PowerShell cmdlets with dynamic parameters specific to Sitecore. These parameters appear based on context:

- **Language** - Specify language versions (`-Language "en-US"` or `-Language *`)
- **Version** - Specify item versions (`-Version 2` or `-Version *`)
- **Database** - Target specific databases when using IDs
- **Query** - Execute Sitecore queries (`query:` or `fast:`)
- **ID** - Work with items by GUID
- **Uri** - Use ItemUri for complete item identification

See the [Retrieving Items](retrieving-items.md) page for detailed parameter documentation.

## Path Formats

SPE supports multiple path formats for flexibility:

```powershell
# Provider path format (recommended)
Get-Item -Path "master:\content\home"

# Sitecore path format (also works)
Get-Item -Path "master:/sitecore/content/home"

# Mixed separators (both work)
Get-Item -Path "master:\content/home"
Get-Item -Path "master:/content\home"
```

Note: The `/sitecore` portion is optional when using provider paths.

## Working with Field Types

SPE provides intelligent handling of Sitecore field types through automated PowerShell properties:

```powershell
$item = Get-Item -Path "master:\content\home"

# DateTime fields return System.DateTime objects
$item.__Created                              # Returns DateTime
$item.__Created = [DateTime]::Now           # Assign DateTime

# Image fields accept item references
$item.Image = Get-Item "master:\media library\logo"

# Link fields accept item references
$item.Link = Get-Item "master:\content\home\page"

# Multilist fields accept arrays of items
$item.RelatedPages = Get-ChildItem "master:\content\home"
```

See [Editing Items](editing-items.md) for comprehensive field type examples.

## Performance Considerations

When working with large datasets, consider these performance tips:

- Use `fast:` queries instead of `Get-ChildItem -Recurse` when possible
- Leverage `BulkUpdateContext` for multiple updates
- Use `SecurityDisabler` judiciously for read operations
- Batch operations and provide progress feedback

See [Best Practices](best-practices.md) for detailed performance guidance.

## See Also

- **[Security](../security/README.md)** - Securing SPE and managing users/roles
- **[Appendix - Common](../appendix/common/README.md)** - Core cmdlet reference
- **[Appendix - Security](../appendix/security/README.md)** - Security cmdlet reference
- **[Remoting](../remoting.md)** - Automate item operations remotely

## Getting Help

```powershell
# List all item-related commands
Get-Command -Noun Item*

# Get help for a specific command
Get-Help Get-Item -Full
Get-Help Get-ChildItem -Examples

# Find commands by capability
Get-Command -Noun Item* | Where-Object { $_.Parameters.Keys -contains "Language" }
```
