---
description: Avoid these common mistakes when learning SPE.
---

# Common Pitfalls for Beginners

Avoid these common mistakes when starting with SPE. Learning from others' errors will save you time and frustration.

## Forgetting Item Editing Context

Items in Sitecore cannot be modified directly. You **must** use the editing context.

### The Problem

- ❌ **Wrong**: `$item["Title"] = "New Title"` (changes won't save!)
- ✅ **Correct**:
  ```powershell
  $item.Editing.BeginEdit()
  $item["Title"] = "New Title"
  $item.Editing.EndEdit()
  ```

### Why It Matters

Without `BeginEdit()` and `EndEdit()`, changes are made to the in-memory object but **never persisted to the database**.

### Best Practice Pattern

Always use try-catch to handle errors properly:

```powershell
foreach($item in $items) {
    $item.Editing.BeginEdit()
    try {
        # Make your changes
        $item["Title"] = "New Title"
        $item["Text"] = "New content"

        # Commit changes
        $item.Editing.EndEdit()
    }
    catch {
        # Roll back on error
        $item.Editing.CancelEdit()
        Write-Error "Failed to update $($item.ItemPath): $_"
    }
}
```

Read more: [Editing Items](../working-with-items/editing-items.md)

## Using Recursion on Large Trees

Using `-Recurse` carelessly can cause performance problems or even crash your session.

### The Problem

```powershell
# DANGEROUS on large content trees!
Get-ChildItem -Path "master:\content" -Recurse
```

### Why It's Problematic

- On large content trees, this can take minutes or hours
- May consume excessive memory
- Can cause session timeouts
- Locks up the browser

### Better Approaches

#### Option 1: Limit the Scope

```powershell
# Recurse only within a specific subtree
Get-ChildItem -Path "master:\content\home\articles" -Recurse
```

#### Option 2: Use Content Search

```powershell
# Much faster for large queries
$parameters = @{
    Index = "sitecore_master_index"
    Criteria = @{Filter = "DescendantOf"; Value = "{110D559F-DEA5-42EA-9C1C-8A5DF7E70EF9}"}
}

$items = Find-Item @parameters | Initialize-Item
```

#### Option 3: Limit Depth

```powershell
$items = Get-ChildItem -Path "master:\content" -Depth 2
```

Read more: [Find-Item](../appendix/indexing/find-item.md) | [Best Practices](../working-with-items/best-practices.md)

## Ignoring Security

SPE is powerful, which makes security critical.

### Common Security Mistakes

#### Mistake 1: Running Untrusted Scripts

- ❌ **NEVER** run scripts from unknown sources in production
- ❌ **NEVER** skip testing in development first
- ✅ Always review and understand scripts before running
- ✅ Use source control for all scripts

#### Mistake 2: Skipping Security Hardening

- ❌ **NEVER** deploy SPE without reviewing security settings
- ❌ **NEVER** install SPE on CD (Content Delivery) servers
- ❌ **NEVER** expose SPE on internet-facing instances

#### Mistake 3: Granting Excessive Permissions

```powershell
# DON'T give everyone access to everything!
# Use role-based access control instead
```

### Security Checklist

Before deploying SPE to any non-development environment:

1. ✅ Review [Security Hardening Guide](../security/README.md)
2. ✅ Complete [Security Checklist](../security/security-checklist.md)
3. ✅ Configure [Security Policies](../security/security-policies.md)
4. ✅ Set up [Users and Roles](../security/users-and-roles.md)
5. ✅ Implement [Session Elevation](../security/session-elevation.md)

{% hint style="danger" %}
**CRITICAL**: SPE should **NEVER** be installed on Content Delivery (CD) servers or be accessible from internet-facing instances.
{% endhint %}

## Confusing PowerShell Comparison Operators

PowerShell uses different operators than most programming languages.

### Comparison Operator Reference

| C#                        | PowerShell    | Description           |
| :------------------------ | :------------ | :-------------------- |
| `==`                      | `-eq`         | Equal to              |
| `!=`                      | `-ne`         | Not equal to          |
| `<`                       | `-lt`         | Less than             |
| `>`                       | `-gt`         | Greater than          |
| `<=`                      | `-le`         | Less than or equal    |
| `>=`                      | `-ge`         | Greater than or equal |
| `&&`                      | `-and`        | Logical AND           |
| <code>&#124;&#124;</code> | `-or`         | Logical OR            |
| `!`                       | `-not` or `!` | Logical NOT           |

### Examples

```powershell
# String comparison (case-insensitive by default)
$name -eq "Michael"          # True for "michael", "MICHAEL", "Michael"
$name -ceq "Michael"         # Case-sensitive: only true for exact match

# Wildcard matching
$item.Name -like "*Sample*"  # Matches "Sample Item", "My Sample", etc.
$item.Name -notlike "*Test*" # Excludes items with "Test" in name

# Regular expressions
$item.Name -match "^Article\d+$"  # Matches "Article1", "Article23", etc.

# Multiple conditions
$item.TemplateName -eq "Article" -and $_."__Updated" -gt $date

# Collection contains
$languages -contains "en"    # Check if array contains value
```

### Common Mistakes

```powershell
# WRONG - Using C# operators
if($count == 0) { }          # Syntax error!

# RIGHT - Using PowerShell operators
if($count -eq 0) { }         # Correct

# WRONG - Case-sensitive by default assumption
if($name -eq "MICHAEL") { }  # Will match "michael"!

# RIGHT - Explicit case-sensitive check
if($name -ceq "MICHAEL") { } # Only matches exact case
```

## Forgetting About Language Versions

Items in Sitecore can have multiple language versions, which affects querying and editing.

### The Problem

```powershell
# This gets the item in the DEFAULT language (usually en)
$item = Get-Item -Path "master:\content\home"
$item["Title"]  # Returns English title
```

### Working with Languages

```powershell
# Get specific language version
$item = Get-Item -Path "master:\content\home" -Language "de-DE"

# Get all language versions
$item = Get-Item -Path "master:\content\home"
$allVersions = Get-ItemField -Item $item -IncludeStandardFields -Name "*" -ReturnType Field

# Edit specific language
$item = Get-Item -Path "master:\content\home" -Language "de-DE"
$item.Editing.BeginEdit()
$item["Title"] = "Deutscher Titel"
$item.Editing.EndEdit()
```

Read more: [Item Languages](../working-with-items/item-languages.md)

## Suppressing Output Incorrectly

Different methods of suppressing output have very different performance characteristics.

### The Problem

```powershell
# SLOW - Out-Null is a cmdlet, adds pipeline overhead
$builder.Append("Text") | Out-Null

# FAST - Redirect to $null
$builder.Append("Text") > $null
```

### Performance Comparison

```powershell
# Slow (don't use for simple suppression)
$list.Add($item) | Out-Null

# Fast (use this instead)
$list.Add($item) > $null
```

## Next Steps

Now that you know what to avoid:

1. **Review your scripts**: Look for these pitfalls in existing code
2. **Practice safely**: Test in development before production
3. **Learn best practices**: Read [Best Practices](../working-with-items/best-practices.md)
4. **Secure your installation**: Complete the [Security Checklist](../security/security-checklist.md)

{% hint style="success" %}
Everyone makes mistakes when learning. The key is learning from them and building better habits!
{% endhint %}
