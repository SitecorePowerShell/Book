---
description: Learn PowerShell command syntax, pipelines, and essential commands.
---

# Commands and Pipelines

Learning PowerShell begins with running your first command. In this section we learn about the basic command syntax, pipelines, and some common commands you should know.

## Command Syntax

**Example:** The following provides an example syntax for a fake command.

```powershell
Get-Something [[-SomeParameter] <sometype[]>] [-AnotherParameter <anothertype>] [-SomeSwitch]
```

### Verb-Noun Pattern

PowerShell commands follow a **Verb-Noun** syntax. Notice that all properly named commands start with a verb such as Get, Set, or Remove and end with a noun such as Item, User, or Role.

{% hint style="info" %}
The noun in the command should be singular even if the command returns more than one object.
{% endhint %}

**Examples:**

- `Get-Item` - Retrieves an item
- `Set-Item` - Modifies an item
- `Remove-Item` - Deletes an item
- `New-Item` - Creates an item
- `Move-Item` - Moves an item

The verbs are considered "approved" if they align with those that Microsoft recommends. See the following URL [https://msdn.microsoft.com/en-us/library/ms714428(v=vs.85).aspx](https://msdn.microsoft.com/en-us/library/ms714428%28v=vs.85%29.aspx) for a list of approved verbs and a brief explanation on why they were chosen. They are intended to be pretty generic so they apply for multiple contexts like the filesystem, registry, and even Sitecore!

### Parameters and Arguments

The parameters follow the command and usually require arguments. In our example above we have a parameter called **SomeParameter** followed by an argument of type **SomeType**. The final parameter **SomeSwitch** is called a switch.

**Parameter types:**

- **Required parameters** - Must be provided
- **Optional parameters** - May be omitted (shown in `[square brackets]`)
- **Positional parameters** - Can be provided without the parameter name (shown with `[[double brackets]]`)
- **Switch parameters** - Boolean flags that enable/disable behavior

{% hint style="info" %}
The brackets surrounding the parameter and the brackets immediately following a type have different meanings. The former has to do with optional usage whereas the latter indicates the data can be an array of objects.
{% endhint %}

### Parameter Examples

**Example**: The following provides possible permutations for the fake command.

```powershell
<#
    All of the parameters in the command are surrounded by square brackets
    indicating they are optional.
#>
Get-Something
```

```powershell
<#
    SomeParameter has double brackets around the parameter name and argument
    indicating the name is optional and when an argument is passed the name
    can be skipped.
#>
Get-Something "data"
```

```powershell
<#
    AnotherParameter has single brackets indicating that the parameter is
    optional. If the argument is used so must the name. The same reasoning
    can be applied to the switch.
#>
Get-Something "data","data2" -AnotherParameter 100 –SomeSwitch
```

### Splatting Parameters

Instead of passing parameters inline, you can "splat" them using a hashtable:

```powershell
# Define parameters in a hashtable
$props = @{
    "SomeParameter" = @("data","data2")
    "AnotherParameter" = 100
    "SomeSwitch" = $true
}

# Splat with @props instead of $props
Get-Something @props
```

**When to use splatting:**

- Commands with many parameters
- Reusable parameter sets
- Conditional parameters
- Improved readability

## Best Practices for Scripts

Allow scripts to be written with the full command and parameter names:

- ✅ **Do** use full command names (not aliases)
- ✅ **Do** use full parameter names (not abbreviations)
- ✅ **Do** specify parameter names explicitly
- ❌ **Avoid** relying on positional parameters in scripts
- ❌ **Avoid** abbreviating parameter names
- ❌ **Avoid** using command aliases (e.g. dir, cd) in scripts

**Example:**

```powershell
# GOOD - explicit and clear
Get-ChildItem -Path "master:\content\home" -Recurse |
    Where-Object { $_.TemplateName -eq "Article" } |
    Select-Object -Property Name, ID

# BAD - abbreviated and unclear
gci "master:\content\home" -r |
    ? { $_.TemplateName -eq "Article" } |
    select Name, ID
```

{% hint style="info" %}
Aliases and abbreviations are fine for interactive use in the Console, but always use full names in saved scripts!
{% endhint %}

## Essential Commands

Some of the most useful commands to learn can be seen in the table below. These come with vanilla PowerShell.

| Command            | Description                                                                         | Example                                             |
| :----------------- | :---------------------------------------------------------------------------------- | :-------------------------------------------------- |
| **Get-Item**       | Returns an object at the specified path                                             | `Get-Item -Path "master:\content\home"`             |
| **Get-ChildItem**  | Returns children at the specified path. Supports recursion                          | `Get-ChildItem -Path "master:\content" -Recurse`    |
| **Get-Help**       | Returns the help documentation for the specified command or document                | `Get-Help Get-Item`                                 |
| **Get-Command**    | Returns a list of commands                                                          | `Get-Command *Item*`                                |
| **ForEach-Object** | Enumerates over the objects passed through the pipeline                             | `$items \| ForEach-Object { $_.Name }`              |
| **Where-Object**   | Enumerates over the objects passed through the pipeline and filters objects         | `$items \| Where-Object { $_.Name -like "*Test*" }` |
| **Select-Object**  | Returns objects from the pipeline with the specified properties and filters objects | `$items \| Select-Object Name, ID`                  |
| **Sort-Object**    | Sorts the pipeline objects with the specified criteria; usually a property name     | `$items \| Sort-Object Name`                        |
| **Get-Member**     | Returns the methods and properties for the specified object                         | `$item \| Get-Member`                               |
| **Measure-Object** | Calculates statistics on objects                                                    | `$items \| Measure-Object`                          |
| **Group-Object**   | Groups objects by property value                                                    | `$items \| Group-Object TemplateName`               |

{% hint style="info" %}
PowerShell was designed so that after learning a few concepts you can get up and running. Once you get past the basics you should be able to understand most scripts included with SPE.
{% endhint %}

## Pipelines

PowerShell supports chaining of commands through a feature called "Pipelines" using the pipe "|". Similar to Sitecore in that you can short circuit the processing of objects using **Where-Object**.

### The Pipeline Concept

The pipeline passes objects from one command to the next:

```powershell
Command1 | Command2 | Command3
```

- **Command1** produces output
- **Command2** receives that output, processes it, and produces new output
- **Command3** receives Command2's output and produces final output

### Current Object Variables

{% hint style="info" %}
The characters `$_` and `$PSItem` represent the current object getting processed in the pipeline.
{% endhint %}

```powershell
Get-ChildItem -Path "master:\content\home" |
    Where-Object { $_.TemplateName -eq "Article" }
    # $_ represents each item as it flows through
```

### Simple Pipeline Examples

**Example:** The following queries a Sitecore item and removes it.

```powershell
# The remove command accepts pipeline input
Get-Item -Path "master:\content\home\sample item" | Remove-Item

# If multiple items are passed through the pipeline each are removed individually
$items | Remove-Item
```

**Example:** The following gets children and displays specific properties.

```powershell
Get-ChildItem -Path "master:\content\home" |
    Select-Object -Property Name, TemplateName, ID
```

### Complex Pipeline Example

PowerShell also comes with a set of useful commands for filtering and sorting. Let's see those in action.

**Example:** The following queries a tree of Sitecore items and returns only those that meet the criteria. The item properties are reduced and then sorted.

```powershell
# Use variables for parameters such as paths to make scripts easier to read
$path = "master:\content\home\"

Get-ChildItem -Path $path -Recurse |
    Where-Object { $_.Name -like "*Sample*" } |
    Select-Object -Property ID, Name, ItemPath |
    Sort-Object -Property Name
```

{% hint style="info" %}
A best practice in PowerShell is to reduce the number of objects passed through the pipeline as far left as possible. While the example would work if the **Sort-Object** command came before **Where-Object**, we will see a performance improvement because the sorting has fewer objects to evaluate. Some commands such as **Get-ChildItem** support additional options for filtering which further improve performance.
{% endhint %}

## Common Pipeline Patterns

### Filtering

```powershell
# Filter by property value
Get-ChildItem -Path "master:\content\home" |
    Where-Object { $_.TemplateName -eq "Sample Item" }
```

```powershell
# Filter by multiple conditions
Get-ChildItem -Path "master:\content\home" |
    Where-Object {
        $_.TemplateName -eq "Sample Item" -and
        $_.PSFields."__Updated".DateTime -gt (Get-Date).AddDays(-7)
    }
```

```powershell
# Filter and negate
Get-ChildItem -Path "master:\content\home" |
    Where-Object { $_.Name -notlike "*Test*" }
```

### Transforming

```powershell
# Select specific properties
Get-ChildItem -Path "master:\content\home" |
    Select-Object Name, TemplateName, ID
```

```powershell
# Create calculated properties
Get-ChildItem -Path "master:\content\home" |
    Select-Object Name,
        @{Name="Updated";Expression={$_.PSFields."__Updated".DateTime}},
        @{Name="Age";Expression={(Get-Date) - $_.PSFields."__Updated".DateTime}}
```

```powershell
$items = Get-ChildItem -Path "master:\content\home"
# Expand single property to array of values
$items | Select-Object -ExpandProperty Name
# Returns: @("Name1", "Name2", "Name3")
```

### Grouping and Aggregating

```powershell
# Count items by template
Get-ChildItem -Path "master:\content\home" -Recurse |
    Group-Object TemplateName |
    Select-Object Name, Count |
    Sort-Object Count -Descending
```

### Sorting

```powershell
# Sort by name
$items | Sort-Object Name

# Sort descending
$items | Sort-Object Name -Descending

# Sort by multiple properties
$items | Sort-Object TemplateName, Name

# Sort by custom expression
$items | Sort-Object { $_.Name.Length }
```

### ForEach Processing

```powershell
# Process each item
Get-ChildItem -Path "master:\content\home" |
    ForEach-Object {
        Write-Host "Processing: $($_.Name)"
        # Do something with $_
    }

# Transform each item
Get-ChildItem -Path "master:\content\home" |
    ForEach-Object { $_.Name.ToUpper() }
```

## Getting Help

Windows PowerShell is bundled with a ton of documentation that could not possibly be included with this book; we can however show you how to access it.

### Help Commands

**Example:** The following examples demonstrate ways to get help…with PowerShell.

```powershell
# Displays all of the about help documents
help about_*

# Displays help documentation on the topic of Splatting
help about_Splatting

# Displays help documentation on the specified command
help Get-Member

# Get detailed help with examples
Get-Help Get-Item -Detailed

# Get just the examples
Get-Help Get-Item -Examples

# Get help online (opens in browser)
Get-Help Get-Item -Online
```

### Finding Commands

```powershell
# List all commands
Get-Command

# Search for commands by name
Get-Command *Item*

# Search for SPE commands
Get-Command | Where-Object { $_.ImplementingType -and $_.ImplementingType.Assembly.GetName().Name -eq "Spe" }

# Get command details
Get-Command Get-Item | Select-Object *
```

### Discovering Properties and Methods

```powershell
# See what properties and methods an object has
$item = Get-Item -Path "master:\content\home"
$item | Get-Member

# Filter to just properties
$item | Get-Member -MemberType Property

# Filter to just methods
$item | Get-Member -MemberType Method
```

{% hint style="info" %}
PowerShell does not include the complete help documentation by default on Windows. Run the command **Update-Help** from an elevated prompt to update the help files to the latest available version. See `help update-help` for more information on the command syntax and details of its use. All of the SPE help documentation is available regardless of running **Update-Help**.
{% endhint %}

## Readable vs Abbreviated Commands

**Example:** The following demonstrates how commands can be written clearly with little confusion on the intent, then how aliases and abbreviations can get in the way. Always think about the developer that comes after you to maintain the code.

```powershell
# Longhand - recommended for scripts
Get-Command -Name ForEach-Object –Type cmdlet |
    Select-Object -ExpandProperty ParameterSets

# Shorthand - OK for interactive use, NOT recommended for scripts
gcm -na foreach-object -ty cmdlet | select -exp parametersets
```

**Why use the longhand?**

- Easier to read
- Self-documenting
- No ambiguity
- Works even if aliases change
- Better for team collaboration

## Common Aliases

While you shouldn't use aliases in scripts, it's helpful to recognize them:

| Alias     | Command          | Description          |
| :-------- | :--------------- | :------------------- |
| `?`       | `Where-Object`   | Filter objects       |
| `%`       | `ForEach-Object` | Process each object  |
| `select`  | `Select-Object`  | Choose properties    |
| `sort`    | `Sort-Object`    | Sort objects         |
| `group`   | `Group-Object`   | Group by property    |
| `measure` | `Measure-Object` | Calculate statistics |
| `gci`     | `Get-ChildItem`  | Get children         |
| `gi`      | `Get-Item`       | Get item             |
| `cd`      | `Set-Location`   | Change directory     |
| `dir`     | `Get-ChildItem`  | List directory       |
| `ls`      | `Get-ChildItem`  | List directory       |

View all aliases:

```powershell
Get-Alias
```

## Next Steps

Now that you understand commands and pipelines:

1. **Learn about providers**: [Providers](providers.md)
2. **Practice with examples**: [Your First Scripts](first-scripts.md)
3. **Avoid common mistakes**: [Common Pitfalls](common-pitfalls.md)
4. **Deepen your knowledge**: [Working with Items](../working-with-items/README.md)

{% hint style="success" %}
The pipeline is PowerShell's superpower! Master it and you'll be able to accomplish complex tasks with surprisingly little code.
{% endhint %}
