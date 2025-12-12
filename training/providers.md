---
description: Understanding PowerShell providers and the Sitecore provider.
---

# Providers

The provider architecture in PowerShell enables a developer to make a command like **Get-Item** interact with the filesystem files and folders, and then interact with the Sitecore CMS items. This is one of the most powerful concepts in PowerShell!

## What is a Provider?

A provider is an adapter that makes a data store look like a drive. Think of it like this:

- **File system provider**: Makes `C:\ ` look like a navigable drive
- **Registry provider**: Makes `HKLM:\` look like a navigable drive
- **Sitecore provider**: Makes `master:\` look like a navigable drive

This means you can use the same commands (`Get-Item`, `Get-ChildItem`, etc.) to work with files, registry keys, and Sitecore items!

## Available Providers

The SPE module implements a new provider that bridges the Windows PowerShell platform with the Sitecore API. The following table demonstrates the use of **Get-Item** for a variety of providers.

| Name            | Drives            | Example                                   |
| :-------------- | :---------------- | :---------------------------------------- |
| **Alias**       | Alias             | `Get-Item -Path alias:\dir`               |
| **Sitecore**    | core, master, web | `Get-Item -Path master:\`                 |
| **Environment** | Env               | `Get-Item -Path env:\HOMEPATH`            |
| **FileSystem**  | C, D, F, H        | `Get-Item -Path c:\Windows`               |
| **Function**    | Function          | `Get-Item -Path function:\prompt`         |
| **Registry**    | HKLM, HKCU        | `Get-Item -Path hklm:\SOFTWARE`           |
| **Variable**    | Variable          | `Get-Item -Path variable:\PSVersionTable` |

## The Sitecore Provider

The most important provider for SPE is named **Sitecore**. This provider gives you access to Sitecore databases.

### Default Provider

The default provider used by the PowerShell Console and ISE is **Sitecore** with the drive set to the **master** database.

```powershell
# When you open the Console, you start here:
PS master:\>
```

### Database Drives

The Sitecore provider exposes these databases as drives:

```powershell
# Master database
Get-Item -Path "master:\content\home"

# Web database
Get-Item -Path "web:\content\home"

# Core database
Get-Item -Path "core:\content"
```

### Path Format

Sitecore provider paths follow this format:

```
database:\path\to\item
```

**Examples:**

```powershell
master:\content\home
master:\content\home\articles\my-article
web:\content
core:\system
```

{% hint style="info" %}
Notice that the Sitecore provider supports **backslashes** (`\`) and forward slashes (`/`) like the Sitecore UI.

- ✅ **Correct**: `master:\content\home`
- ✅ **Correct**: `master:/content/home`
- ❌ **Wrong**: `/sitecore/content/home`
  {% endhint %}

## Switching Between Providers

**Example:** The following demonstrates switching between providers using the function **cd**, an alias for **Set-Location**, while in the Console.

```powershell
PS master:\> cd c:\
PS C:\> cd hklm:
PS HKLM:\> cd env:
PS Env:\> cd master:
PS master:\>
```

{% hint style="warning" %}
You may have noticed that the C drive is the only path in which a backslash was used before changing drives. Leaving off the backslash will result in the path changing to C:\windows\system32\inetsrv. This similar behavior can be experienced while in the Windows PowerShell Console, where the path is changed to C:\Windows\System32.
{% endhint %}

## Working with the Sitecore Provider

### Getting Items

```powershell
# Get a single item
$home = Get-Item -Path "master:\content\home"

# Get children
$children = Get-ChildItem -Path "master:\content\home"

# Get all descendants
$all = Get-ChildItem -Path "master:\content\home" -Recurse
```

### Creating Items

```powershell
# Create a new item
New-Item -Path "master:\content\home" -Name "My New Item" -ItemType "Sample/Sample Item"
```

### Removing Items

```powershell
# Remove an item (goes to Recycle Bin)
Remove-Item -Path "master:\content\home\test-item"
```

### Testing if Item Exists

```powershell
# Check if an item exists
if(Test-Path -Path "master:\content\home\my-item") {
    Write-Host "Item exists!"
}
```

## Provider-Specific Features

While all providers support basic operations like `Get-Item` and `Get-ChildItem`, some have special features:

### Sitecore Provider Features

```powershell
# Get item with specific language
Get-Item -Path "master:\content\home" -Language "de-DE"

# Get specific version
Get-Item -Path "master:\content\home" -Version 5

# Include standard fields
Get-Item -Path "master:\content\home" | Get-ItemField -IncludeStandardFields
```

### File System Provider Features

```powershell
# Get files with filter
Get-ChildItem -Path "C:\temp" -Filter "*.txt"

# Get files with specific attributes
Get-ChildItem -Path "C:\temp" -File
Get-ChildItem -Path "C:\temp" -Directory
```

## Why Providers Matter

Understanding providers is crucial because:

1. **Same commands work everywhere** - Learn `Get-Item` once, use it for files, registry, and Sitecore
2. **Path-based navigation** - Everything has a path you can navigate
3. **Consistent patterns** - If it works on files, it probably works on Sitecore items
4. **Powerful automation** - Combine providers to move data between systems

### Example: Cross-Provider Automation

```powershell
# Get Sitecore items
$items = Get-ChildItem -Path "master:\content\home\articles"

# Export to file system
$items | Select-Object Name, TemplateName |
    Export-Csv -Path "$($SitecoreDataFolder)\articles.csv" -NoTypeInformation
```

## Common Provider Operations

| Operation         | File System                       | Sitecore                                |
| :---------------- | :-------------------------------- | :-------------------------------------- |
| **Get item**      | `Get-Item C:\file.txt`            | `Get-Item master:\content\home`         |
| **List children** | `Get-ChildItem C:\`               | `Get-ChildItem master:\content`         |
| **Create**        | `New-Item C:\file.txt`            | `New-Item master:\content\home\item`    |
| **Remove**        | `Remove-Item C:\file.txt`         | `Remove-Item master:\content\home\item` |
| **Test exists**   | `Test-Path C:\file.txt`           | `Test-Path master:\content\home\item`   |
| **Move**          | `Move-Item C:\old.txt C:\new.txt` | `Move-Item master:\a master:\b`         |
| **Copy**          | `Copy-Item C:\old.txt C:\new.txt` | `Copy-Item master:\a master:\b`         |

## Learn More

This introduction covers the basics of the provider architecture and how to use paths in SPE.

For comprehensive details about working with the Sitecore provider (item fields, languages, versions, wildcards, performance optimization, and advanced techniques), see the complete guide: [Working with Items - Providers](../working-with-items/providers.md)

## Next Steps

Now that you understand providers:

1. **Practice with examples**: [Your First Scripts](first-scripts.md)
2. **Avoid common mistakes**: [Common Pitfalls](common-pitfalls.md)
3. **Deep dive into items**: [Working with Items](../working-with-items/README.md)
4. **Read comprehensive provider docs**: [Providers](../working-with-items/providers.md)
