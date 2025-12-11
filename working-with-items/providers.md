# Providers

PowerShell providers expose data stores through a consistent interface that resembles a file system. SPE extends this model with the CmsItemProvider, enabling you to navigate and manipulate Sitecore content as if it were a file system.

## Understanding Providers

Run `Get-PSProvider` to see all available providers in your SPE session:

```powershell
Get-PSProvider

Name        Capabilities                           Drives
----        ------------                           ------
Registry    ShouldProcess, Transactions            {HKLM, HKCU}
Alias       ShouldProcess                          {Alias}
Environment ShouldProcess                          {Env}
FileSystem  Filter, ShouldProcess, Credentials     {C}
Function    ShouldProcess                          {Function}
Variable    ShouldProcess                          {Variable}
Certificate ShouldProcess                          {Cert}
WSMan       Credentials                            {WSMan}
Sitecore    Filter, ExpandWildcards, ShouldProcess {master, web, core}
```

## Built-In Providers

### Sitecore

The Sitecore is the heart of SPE's Sitecore integration. It exposes Sitecore databases as PowerShell drives.

**Features:**

- Navigate Sitecore content trees using paths
- Access items by ID, query, or URI
- Support for language and version parameters
- Filter by template and other properties

**Example:** Explore the CmsItemProvider.

```powershell
Get-PSProvider -PSProvider Sitecore

Name     Capabilities                           Drives
----     ------------                           ------
Sitecore Filter, ExpandWildcards, ShouldProcess {master, web, core}
```

### FileSystem Provider

The FileSystem provider allows interaction with the server's file system.

**Example:** Access file system from SPE.

```powershell
# List files in the Data folder
Get-ChildItem -Path "$SitecoreDataFolder\logs"

# Read a log file
Get-Content -Path "$SitecoreLogFolder\log.txt" -Tail 50

# Create a temp file
New-Item -Path "$SitecoreTempFolder\myfile.txt" -ItemType File -Value "Content"
```

## PowerShell Drives

Drives are the access points for providers. SPE creates drives for each Sitecore database.

### Sitecore Database Drives

**Example:** List all available drives.

```powershell
Get-PSDrive

ame     Used (GB) Free (GB) Provider    Root                        CurrentLocation
----     --------- --------- --------    ----                        ---------------
Alias                        Alias
C             0.42    126.46 FileSystem  C:\                windows\system32\inetsrv
Cert                         Certificate \
core                         Sitecore    core:
Env                          Environment
Function                     Function
HKCU                         Registry    HKEY_CURRENT_USER
HKLM                         Registry    HKEY_LOCAL_MACHINE
master                       Sitecore    master:                        content\Home
Variable                     Variable
web                          Sitecore    web:
WSMan                        WSMan
```

### Drive Structure

Each Sitecore database drive has the following structure:

```
master:\
├── content\
├── layout\
├── media library\
├── system\
└── templates\
```

The root of each drive represents `/sitecore` in Sitecore paths.

{% hint style="info" %}
The text `/sitecore` or `\sitecore` is typically omitted when using the drive names such as `core:`, `master:` and `web:`.
{% endhint %}

## Navigating Between Providers

Use `cd` or `Set-Location` to switch between drives and providers.

**Example:** Navigate between Sitecore databases.

```powershell
# Start in master
PS master:\>

# Switch to core database
PS master:\> cd core:
PS core:\>

# Switch to web database
PS core:\> cd web:
PS web:\>

# Return to master
PS web:\> Set-Location -Path master:
PS master:\>
```

**Example:** Navigate between providers.

```powershell
# Switch to file system
PS master:\> cd C:\
PS C:\>

# Return to master database
PS C:\> cd master:
PS master:\>
```

{% hint style="warning" %}
When using the FileSystem provider, include the backslash (e.g., `C:\`) to access the root of the drive. Due to w3wp.exe behavior, `cd C:` may not behave as expected.
{% endhint %}

## Path Formats

The `Sitecore` provider supports multiple path formats for flexibility.

### Provider Path Format (Recommended)

```powershell
Get-Item -Path "master:\content\home"
```

### Sitecore Path Format

```powershell
Get-Item -Path "master:/sitecore/content/home"
```

### Mixed Separators

Both forward and backward slashes work interchangeably:

```powershell
Get-Item -Path "master:\content/home"
Get-Item -Path "master:/content\home"
```

### Relative Paths

Use relative paths when you're already in a specific location:

```powershell
PS master:\content> Get-Item -Path ".\home"
PS master:\content> cd home
PS master:\content\home> Get-Item -Path ".."  # Returns parent (\content)
```

## Provider-Specific Features

### Dynamic Parameters

When using the `Sitecore` provider, cmdlets gain Sitecore-specific dynamic parameters:

**Example:** Using dynamic parameters.

```powershell
# Language parameter (only available with Sitecore provider)
Get-Item -Path "master:\content\home" -Language "da"

# ID parameter (only available with Sitecore provider)
Get-Item -Path "master:" -ID "{110D559F-DEA5-42EA-9C1C-8A5DF7E70EF9}"

# Query parameter (only available with Sitecore provider)
Get-Item -Path "master:" -Query "/sitecore/content//*[@@templatename='Sample Item']"
```

### Property Access

The `Sitecore` provider exposes Sitecore item properties through PowerShell properties:

**Example:** Access item properties.

```powershell
$item = Get-Item -Path "master:\content\home"

# Standard properties
$item.Name
$item.ID
$item.TemplateName
$item.ItemPath
$item.Language
$item.Version

# Field values
$item.Title
$item.Text
$item.__Created
$item.__Updated
```

## Common Provider Patterns

### Pattern: Work with Multiple Databases

```powershell
$databases = @("master", "core", "web")

foreach($db in $databases) {
    $itemCount = (Get-ChildItem -Path "$($db):\content" -Recurse).Count
    Write-Host "$db database: $itemCount items"
}
```

### Pattern: Compare Items Across Databases

```powershell
$masterItem = Get-Item -Path "master:\content\home"
$webItem = Get-Item -Path "web:\content\home"

if ($masterItem.__Updated -gt $webItem.__Updated) {
    Write-Host "Master is newer than Web"
} else {
    Write-Host "Web is up to date"
}
```

### Pattern: File System Operations

```powershell
# Export items to file system
$items = Get-ChildItem -Path "master:\content\home" -Recurse
$report = $items | Select-Object Name, Template Name, ItemPath

$report | Export-Csv -Path "C:\reports\items.csv" -NoTypeInformation

# Read configuration files
$configPath = Join-Path $SitecoreDataFolder "serialization\config"
Get-ChildItem -Path $configPath -Filter "*.yml"
```

### Pattern: Working Directory Management

```powershell
# Save current location
Push-Location

# Do work in another location
cd "master:\templates"
# ... perform operations ...

# Return to previous location
Pop-Location
```

### Pattern: Cross-Provider Operations

```powershell
# Export Sitecore items to JSON files
$items = Get-ChildItem -Path "master:\content\data" -Recurse

foreach($item in $items) {
    $data = [PSCustomObject]@{
        Name = $item.Name
        ID = $item.ID
        Template = $item.TemplateName
        Title = $item.Title
        Text = $item.Text
    }

    $fileName = "$($item.ID).json"
    $filePath = Join-Path $SitecoreTempFolder $fileName

    $data | ConvertTo-Json | Set-Content -Path $filePath
    Write-Host "Exported: $fileName"
}
```

## Provider Cmdlets

Common cmdlets work across all providers:

| Cmdlet                 | Purpose               | Example                                 |
| ---------------------- | --------------------- | --------------------------------------- |
| `Get-Location` / `pwd` | Show current location | `Get-Location`                          |
| `Set-Location` / `cd`  | Change location       | `cd master:\content`                    |
| `Push-Location`        | Save location         | `Push-Location`                         |
| `Pop-Location`         | Restore location      | `Pop-Location`                          |
| `Get-Item`             | Get item at path      | `Get-Item -Path "master:\content\home"` |
| `Get-ChildItem` / `ls` | List children         | `Get-ChildItem -Path "master:\content"` |
| `Test-Path`            | Check if path exists  | `Test-Path "master:\content\home"`      |

### Testing Paths

**Example:** Verify item existence.

```powershell
if (Test-Path "master:\content\home") {
    Write-Host "Item exists"
    $item = Get-Item -Path "master:\content\home"
} else {
    Write-Host "Item not found"
}
```

**Example:** Conditional operations based on existence.

```powershell
$targetPath = "master:\content\import"

if (-not (Test-Path $targetPath)) {
    New-Item -Path "master:\content" -Name "import" -ItemType "Common/Folder"
    Write-Host "Created import folder"
}

# Now safe to use
$importFolder = Get-Item -Path $targetPath
```

## Advanced Provider Usage

### Using Provider Paths

Get the full provider path for an item:

**Example:** Access provider path.

```powershell
$item = Get-Item -Path "master:\content\home"
$item.ProviderPath

# master:\content\home
```

### Custom Drive Creation

While rare, you can create custom drives:

**Example:** Create a drive pointing to a specific location.

```powershell
New-PSDrive -Name "content" -PSProvider Sitecore -Root "master:\content"

# Now you can use:
Get-Item -Path "content:\home"
```

### Provider Capabilities

Check what a provider can do:

**Example:** View provider capabilities.

```powershell
(Get-PSProvider -PSProvider Sitecore).Capabilities

# Filter, ExpandWildcards, ShouldProcess
```

## Troubleshooting

### Issue: Dynamic Parameters Not Appearing

**Symptom:** Parameters like `-Language` or `-ID` are not available.

**Solution:** Ensure you're specifying a database path:

```powershell
# WRONG - no database specified
Get-Item -ID "{110D559F-DEA5-42EA-9C1C-8A5DF7E70EF9}"

# CORRECT - database specified
Get-Item -Path "master:" -ID "{110D559F-DEA5-42EA-9C1C-8A5DF7E70EF9}"
```

### Issue: File System Path Behavior

**Symptom:** `cd C:` doesn't go to C:\ root.

**Solution:** Always include the backslash for FileSystem provider root:

```powershell
# WRONG
cd C:

# CORRECT
cd C:\
```

### Issue: Path Not Found

**Symptom:** "Cannot find path" error.

**Solution:** Verify the item exists and the path is correct:

```powershell
# Check if path exists
if (Test-Path "master:\content\home") {
    $item = Get-Item -Path "master:\content\home"
} else {
    Write-Host "Path not found. Check spelling and database."
}
```

## Performance Considerations

### Provider vs. Direct API

The `Sitecore` provider adds convenience but may have overhead for large operations:

```powershell
# Provider-based (convenient)
$items = Get-ChildItem -Path "master:\content" -Recurse

# Direct API (faster for large datasets)
$db = Get-Database -Name "master"
$root = $db.GetItem("/sitecore/content")
$items = $root.Axes.GetDescendants() | Initialize-Item
```

### Caching

Providers may cache data. If you're seeing stale data:

```powershell
# Clear caches
$db = Get-Database -Name "master"
$db.Caches.ItemCache.Clear()
$db.Caches.DataCache.Clear()
```

## See Also

- [Variables](variables.md) - Built-in variables for paths
- [Retrieving Items](retrieving-items.md) - Finding items using providers
- [Best Practices](best-practices.md) - Performance optimization

## References

- [PowerShell Providers Documentation](https://docs.microsoft.com/en-us/powershell/scripting/developer/provider/windows-powershell-provider-overview)
- [Issue #314 - FileSystem Provider Behavior](https://github.com/SitecorePowerShell/Console/issues/314)
