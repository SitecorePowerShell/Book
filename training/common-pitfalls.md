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

- ❌ **NEVER** run scripts from unknown sources
- ❌ **NEVER** deploy SPE without reviewing security settings
- ❌ **NEVER** install SPE on CD (Content Delivery) servers
- ✅ Always review and understand scripts before running
- ✅ Use role-based access control
- ✅ Test in development first

{% hint style="danger" %}
Review the [Security Hardening Guide](../security/README.md) and complete the [Security Checklist](../security/security-checklist.md) before deploying to any non-development environment.
{% endhint %}

## Confusing PowerShell Comparison Operators

PowerShell uses different operators than most programming languages. See [Language Basics](language-basics.md#comparisons) for the complete comparison operator reference.

### Examples

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

Learn more: [Language Basics - Comparisons](language-basics.md#comparisons)

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
