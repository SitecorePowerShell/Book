---
description: Quick start guide to get up and running with SPE.
---

# Getting Started

Ready to start using SPE? This guide will get you up and running in minutes.

## What You Need

Before starting, ensure you have:

- ✅ SPE installed in your Sitecore instance → [Installation Guide](../installation/README.md)
- ✅ Appropriate permissions to access PowerShell tools → [Security](../security/README.md)
- ✅ Basic understanding of Sitecore concepts (items, templates, fields)

{% hint style="info" %}
You don't need to know PowerShell or programming to get started! This guide is designed for Sitecore users who want to automate tasks.
{% endhint %}

## Opening the PowerShell Interfaces

SPE provides two main interfaces for running scripts:

### The Console - Quick Commands

The **PowerShell Console** is perfect for running quick, one-off commands.

**How to open:**

1. Log into Sitecore
2. Go to **Desktop** (bottom left icon)
3. Click **PowerShell Console**

![Console Interface](../.gitbook/assets/cli-empty.png)

**When to use the Console:**

- Running single commands
- Quick queries and checks
- Interactive exploration
- Testing command syntax

### The ISE - Script Development

The **PowerShell ISE** (Integrated Scripting Environment) is a full-featured script editor.

**How to open:**

1. Log into Sitecore
2. Go to **Desktop** (bottom left icon)
3. Click **Development Tools**
4. Click **PowerShell ISE**

![ISE Interface](../.gitbook/assets/ise-empty.png)

**When to use the ISE:**

- Writing multi-line scripts
- Developing reusable tools
- Saving scripts for later use
- Debugging complex logic

Learn more: [Console](../interfaces/console.md) | [ISE](../interfaces/scripting.md)

## Your First Command

Let's run your very first SPE command!

### Step 1: Open the Console

Follow the instructions above to open the PowerShell Console.

### Step 2: Run a Command

Type this command and press Enter:

```powershell
Get-Item -Path "master:\content\home"
```

### Step 3: Examine the Results

You should see output showing details about your Home item:

```powershell
Name Children Language Version Id                                     TemplateName
---- -------- -------- ------- --                                     ------------
Home False    en       1       {110D559F-DEA5-42EA-9C1C-8A5DF7E70EF9} Sample Item
```

{% hint style="success" %}
**Congratulations!** You just retrieved a Sitecore item using PowerShell. This is the foundation of everything you'll do with SPE.
{% endhint %}

## Understanding What Just Happened

Let's break down that command:

```powershell
Get-Item -Path "master:\content\home"
```

- **Get-Item** - The command (cmdlet) that retrieves an item
- **-Path** - A parameter that specifies which item to get
- **"master:\content\home"** - The location of the item
  - `master:` - The database
  - `\content\home` - The path to the item

## Try These Next

Now that you've run your first command, try these:

### List Children of Home

```powershell
Get-ChildItem -Path "master:\content\home"
```

This shows all direct children of the Home item.

### List All Descendants

```powershell
Get-ChildItem -Path "master:\content\home" -Recurse
```

{% hint style="warning" %}
Be careful with `-Recurse` on large content trees! It can take a long time. See [Common Pitfalls](common-pitfalls.md) for more.
{% endhint %}

### Get Item Properties

PowerShell has a default set of properties to format to the output window even though the objects returned still contain all the properties.

```powershell
Get-Item -Path "master:\content\home" | Select-Object -Property Name, ID, TemplateName
```

Using `Select-Object`, the properties are trimmed and the output window reflects the specific fields.

**Output:**

```powershell
Name ID                                     TemplateName
---- --                                     ------------
Home {110D559F-DEA5-42EA-9C1C-8A5DF7E70EF9} Sample Item
```

### Find Items by Name

```powershell
Get-ChildItem -Path "master:\content\home" | Where-Object { $_.Name -like "*Sample*" }
```

This finds all items under Home that have "Sample" in their name.

## Using the ISE

The ISE is more powerful for writing longer scripts. Let's try it out.

### Step 1: Open the ISE

Follow the instructions above to open the PowerShell ISE.

### Step 2: Write a Script

In the top pane (script editor), type:

```powershell
# Get the home item
$homeItem = Get-Item -Path "master:\content\home"

# Show its children
$children = Get-ChildItem -Path $homeItem.ProviderPath

# Display count
Write-Host "Home has $($children.Count) children" -ForegroundColor Green

# Show the list
$children | Select-Object Name, TemplateName
```

### Step 3: Execute the Script

Click the **Execute** button (▶️) or press **Ctrl-E**.

### Step 4: View Results

The bottom pane shows the output of your script.

{% hint style="info" %}
The ISE provides **IntelliSense** - start typing a command and press Ctrl+Space to see suggestions!
{% endhint %}

## Key Concepts

### Variables

Variables store data and start with `$`:

```powershell
$item = Get-Item -Path "master:\content\home"
$name = $item.Name
$count = 5
```

Learn more: [Variables](../working-with-items/variables.md)

### Pipelines

The pipe `|` chains commands together:

```powershell
Get-ChildItem -Path "master:\content\home" |
    Where-Object { $_.TemplateName -eq "Sample Item" } |
    Select-Object Name, ID
```

Learn more: [Commands and Pipelines](commands-and-pipelines.md)

### Providers

SPE uses providers to access different data stores. The most important is the **Sitecore provider**:

```powershell
# Sitecore databases
Get-Item -Path "master:\content\home"
Get-Item -Path "web:\content\home"
Get-Item -Path "core:\content"

# File system
Get-Item -Path "C:\temp"

# Registry
Get-Item -Path "HKLM:\SOFTWARE"
```

Learn more: [Providers](providers.md)

## Common Tasks

Here are some common tasks you can do right now:

### Check if Item Exists

```powershell
if(Test-Path "master:\content\home\test") {
    Write-Host "Item exists!" -ForegroundColor Green
} else {
    Write-Host "Item not found" -ForegroundColor Yellow
}
```

### Count Items by Template

```powershell
$items = Get-ChildItem -Path "master:\content\home" -Recurse
$items | Group-Object TemplateName | Select-Object Name, Count | Sort-Object Count -Descending
```

### Find Items Modified Recently

```powershell
$threshold = (Get-Date).AddDays(-7)
Get-ChildItem -Path "master:\content\home" -Recurse |
    Where-Object { $_."__Updated" -gt $threshold } |
    Select-Object Name, ItemPath, "__Updated"
```

### Export Data to CSV

```powershell
$items = Get-ChildItem -Path "master:\content\home"
$items | Select-Object Name, TemplateName, "__Updated" |
    Export-Csv -Path "$($SitecoreDataFolder)\export\items.csv" -NoTypeInformation
```

## Getting Help

SPE includes extensive built-in help:

### Get Help for a Command

```powershell
Get-Help Get-ItemAcl
```

### Get Detailed Help

```powershell
Get-Help Read-Variable -Detailed
```

### Get Examples

```powershell
Get-Help Find-Item -Examples
```

### List All SPE Commands

```powershell
Get-Command | Where-Object { $_.ImplementingType -and $_.ImplementingType.Assembly.GetName().Name -eq "Spe" }
```

Learn more: [Help](../interfaces/help.md)

## Script Library

The ISE includes a **Script Library** with ready-to-use examples:

1. Open the ISE
2. Click **Open** in the ribbon
3. Click to expand **SPE** in the tree
4. Expand folders to find examples:
   - **Training** - Learning exercises
   - **Reports** - Content reports
   - **Tools** - Utility scripts

{% hint style="tip" %}
The Script Library is a great way to learn! Read through examples to see how different commands work together.
{% endhint %}

## Safety Tips

Before you continue, remember these important safety rules:

### Always Test First

- ❌ **Don't** run scripts in production without testing
- ✅ **Do** test in development first
- ✅ **Do** understand what a script does before running it

### Be Careful with Modifications

Commands that change data:

- `Remove-Item` - Deletes items (can be recovered from Recycle Bin)
- `Set-Item` - Modifies items
- `Move-Item` - Moves items
- `Publish-Item` - Publishes changes to web database

### Use Version Control

- Save important scripts to source control
- Document what your scripts do
- Share scripts with your team

### Review Security

{% hint style="danger" %}
**IMPORTANT**: SPE is powerful and requires proper security configuration.

- Never run untrusted scripts
- Review [Security Guide](../security/README.md) before deploying
- Follow the [Security Checklist](../security/security-checklist.md)
  {% endhint %}

## Next Steps

Now that you're up and running:

1. **Learn the syntax**: [Language Basics](language-basics.md)
2. **Master commands**: [Commands and Pipelines](commands-and-pipelines.md)
3. **Understand providers**: [Providers](providers.md)
4. **Write your first scripts**: [Your First Scripts](first-scripts.md)
5. **Avoid mistakes**: [Common Pitfalls](common-pitfalls.md)

## Quick Reference

### Essential Commands

| Command            | Description                                                                          |
| :----------------- | :----------------------------------------------------------------------------------- |
| **Get-Item**       | Returns an object at the specified path.                                             |
| **Get-ChildItem**  | Returns children at the specified path. Supports recursion.                          |
| **Get-Help**       | Returns the help documentation for the specified command or document.                |
| **Get-Command**    | Returns a list of commands.                                                          |
| **ForEach-Object** | Enumerates over the objects passed through the pipeline.                             |
| **Where-Object**   | Enumerates over the objects passed through the pipeline and filters objects.         |
| **Select-Object**  | Returns objects from the pipeline with the specified properties and filters objects. |
| **Sort-Object**    | Sorts the pipeline objects with the specified criteria; usually a property name.     |
| **Get-Member**     | Returns the methods and properties for the specified object.                         |

### Path Format

```
database:\path\to\item

Examples:
master:\content\home
web:\content\home\article
core:\content
```

### Special Variables

```powershell
$_          # Current item in pipeline
$PSItem     # Same as $_
$true       # Boolean true
$false      # Boolean false
$null       # Null value
```

Ready to dive deeper? Continue with [Language Basics](language-basics.md)!
