---
description: Learn PowerShell syntax by comparing it to C#.
---

# Language Basics

PowerShell is built on the Microsoft .NET technology; you will find that most APIs in your libraries can be accessed from within the PowerShell runtime. In this section we will see similarities between the C# and PowerShell syntax.

If you're not familiar with C#, don't worry! The examples below show both languages side-by-side so you can see the patterns.

## Variables

**Note:** Variables in PowerShell are denoted by the `$` character followed by the name. You will see this through the examples below.

```csharp
// C# - declare and assign
var name = "Michael";
string title = "Developer";
int count = 5;
```

```powershell
# PowerShell - assign (type is inferred)
$name = "Michael"
$title = "Developer"
$count = 5

# Optionally specify type
[string]$title = "Developer"
[int]$count = 5
```

## C# to PowerShell Translation

Use the tables below to aid in translating from C# to PowerShell. Some of the examples below are not "exact" translations, but should give you a good idea on what it would look like.

```csharp
// Perform simple math in C#
var total = 1 + 1;
total += 2;
var result = total * 3 / 2;
```

```powershell
# Perform simple math in PowerShell
$total = 1 + 1
$total += 2
$result = $total * 3 / 2
```

**Operators are the same:**

- Addition: `+`
- Subtraction: `-`
- Multiplication: `*`
- Division: `/`
- Modulus: `%`

### Arrays

Working with Dynamic and Fixed dimensional arrays.

```csharp
/*
  Create a new dynamic list of strings in C#
*/
var names = new List<string>();
names.Add("Michael");
names.Add("Adam");
```

```powershell
<#
  Create a new fixed list of strings in PowerShell
#>
[string[]]$names = @()
$names += "Michael"
$names += "Adam"
```

{% hint style="warning" %}
Using `+=` with arrays in PowerShell is slow because it creates a new array each time. See [Performance Considerations](#performance-considerations) for better alternatives.
{% endhint %}

### Hashtables

Working with hashtables (dictionaries).

```csharp
// Create a hashtable of data in C#
var table = new Hashtable();
table["Name"] = "Michael";
table["Age"] = 33;
```

```powershell
# Create a new hashtable of data in PowerShell
$table = @{}
$table["Name"] = "Michael"
$table["Age"] = 33

# Or create inline
$table = @{
    Name = "Michael"
    Age = 33
}
```

### Ordered Dictionaries

Working with dictionaries that preserve insertion order.

```csharp
// Ordered Dictionary in C#
var od = new OrderedDictionary();
od.Add("z","Last Letter");
od.Add("a","First Letter");
```

```powershell
# Ordered Dictionary in PowerShell
$od = [ordered]@{}
$od.Add("z","Last Letter")
$od.Add("a","First Letter")
```

```csharp
// Check if the string is null or empty using a static method in C#
if(string.IsNullOrEmpty(name)) {
  ...
}
else {
  ...
}
```

```powershell
# Check if the string is null or empty using a static method in PowerShell
if([string]::IsNullOrEmpty($name)) {
  ...
} else {
  ...
}
```

**Switch statements:**

```csharp
// C# switch
switch(value) {
    case "A":
        DoSomething();
        break;
    case "B":
        DoSomethingElse();
        break;
    default:
        DoDefault();
        break;
}
```

```powershell
# PowerShell switch
switch($value) {
    "A" {
        Do-Something
    }
    "B" {
        Do-SomethingElse
    }
    default {
        Do-Default
    }
}
```

**Important:** PowerShell uses different comparison operators than C#!

```csharp
// Compare values in C#
name == "Michael"
total <= 3
names.Count() > 2 && name[0] != "Adam"
```

```powershell
# Compare values in PowerShell
$name -eq "Michael"
$total -le 3
$names.Count -gt 2 -and $name[0] -ne "Adam"
```

### Comparison Operators

| C#   | PowerShell | Description           |
| :--- | :--------- | :-------------------- |
| `==` | `-eq`      | Equal to              |
| `!=` | `-ne`      | Not equal to          |
| `<`  | `-lt`      | Less than             |
| `>`  | `-gt`      | Greater than          |
| `<=` | `-le`      | Less than or equal    |
| `>=` | `-ge`      | Greater than or equal |

### Logical Operators

| C#                        | PowerShell    | Description |
| :------------------------ | :------------ | :---------- |
| `&&`                      | `-and`        | Logical AND |
| <code>&#124;&#124;</code> | `-or`         | Logical OR  |
| `!`                       | `-not` or `!` | Logical NOT |

### Pattern Matching

```powershell
# Case-insensitive (default)
$name -eq "Michael"      # Matches "michael", "MICHAEL", "Michael"

# Case-sensitive
$name -ceq "Michael"     # Only matches exact case

# Wildcard matching
$name -like "*Smith"     # Matches "John Smith", "Jane Smith"
$name -notlike "*Test*"  # Excludes items with "Test"

# Regular expressions
$name -match "^\d{3}"    # Matches names starting with 3 digits
```

{% endhint style="info" %}
Most comparisons in PowerShell are case-insensitive by default. Use operators starting with `c` (like `-ceq`) for case-sensitive comparisons.
{% endhint %}

```csharp
// Negate value in C#
var isTrue = !false;
var isNotEmpty = !string.IsNullOrEmpty(value);
```

```powershell
# Negate value in PowerShell
$isTrue = !$false
$isTrue = -not $false

$isNotEmpty = ![string]::IsNullOrEmpty($value)
$isNotEmpty = -not [string]::IsNullOrEmpty($value)
```

**Both work the same:**

- `!$value` - Shorter syntax
- `-not $value` - More explicit

### String Interpolation

```csharp
// String interpolation in C#
var message = $"Hello, {name}";
var result = $"Total: {count * 2}";
```

```powershell
# String interpolation in PowerShell
$message = "Hello, $name"
$result = "Total: $($count * 2)"
```

{% hint style="info" %}
Use `$()` for expressions: `"Result: $(2 + 2)"` → "Result: 4"
{% endhint %}

### Escape Characters

Escape double quotes in string. Alternatively you can use a single quote.

```csharp
// Escape characters in C#
string message = "They said, \"SPE is the greatest!\".";
message = 'I said, "Thanks!"';
message = "We celebrated together, 'Go SPE!'";
```

```powershell
# Escape characters in PowerShell
$message = "They said, `"SPE is the greatest!`"."
$message = 'I said, "Thanks!".'
$message = "We celebrated together, 'Go SPE!'."
```

**Escape character:** PowerShell uses backtick `` ` `` instead of backslash `\`

### Multi-line Strings

```csharp
// C# multi-line
var text = @"
Line 1
Line 2
Line 3
";
```

```powershell
# PowerShell here-string
$text = @"
Line 1
Line 2
Line 3
"@
```

### String Operations

```powershell
# Concatenation
$fullName = $firstName + " " + $lastName
$fullName = "$firstName $lastName"

# Length
$length = $name.Length

# Substring
$sub = $name.Substring(0, 5)

# Replace
$updated = $name.Replace("old", "new")

# Split
$parts = $name.Split(" ")

# Join
$joined = $parts -join ", "

# To upper/lower
$upper = $name.ToUpper()
$lower = $name.ToLower()
```

### For Loop

```csharp
// C# for loop
for(int i = 0; i < 10; i++) {
    Console.WriteLine(i);
}
```

```powershell
# PowerShell for loop
for($i = 0; $i -lt 10; $i++) {
    Write-Host $i
}
```

### ForEach Loop

```csharp
// C# foreach
foreach(var item in items) {
    ProcessItem(item);
}
```

```powershell
# PowerShell foreach
foreach($item in $items) {
    Process-Item $item
}
```

### While Loop

```csharp
// C# while
while(count < 100) {
    count++;
}
```

```powershell
# PowerShell while
while($count -lt 100) {
    $count++
}
```

### Accessing .NET Types

```csharp
// Access static property in C#
var today = DateTime.Today;
var guid = Guid.NewGuid();
```

```powershell
# Access static property in PowerShell
$today = [datetime]::Today
$guid = [guid]::NewGuid()
```

### Common .NET Types

```powershell
# DateTime
$now = [datetime]::Now
$today = [datetime]::Today
$parsed = [datetime]::Parse("2024-01-15")
$custom = Get-Date -Year 2024 -Month 1 -Day 15

# String
$empty = [string]::Empty
$isNullOrEmpty = [string]::IsNullOrEmpty($value)
$joined = [string]::Join(", ", $array)

# Math
$max = [math]::Max(5, 10)
$min = [math]::Min(5, 10)
$rounded = [math]::Round(3.7)

# Guid
$newGuid = [guid]::NewGuid()
$parsed = [guid]::Parse("{110D559F-DEA5-42EA-9C1C-8A5DF7E70EF9}")

# Path
$combined = [System.IO.Path]::Combine("C:\temp", "file.txt")
$extension = [System.IO.Path]::GetExtension("file.txt")
```

### Creating .NET Objects

```csharp
// C# - create object
var list = new List<string>();
var dict = new Dictionary<string, int>();
```

```powershell
# PowerShell - create object
$list = New-Object System.Collections.Generic.List[string]
$dict = New-Object 'System.Collections.Generic.Dictionary[string,int]'

# Alternative syntax
$list = [System.Collections.Generic.List[string]]::new()
```

## Comments

```csharp
// C# - Single line comment

/*
  C# - Multi-line comment
  More text here
*/
```

```powershell
# PowerShell - Single line comment

<#
  PowerShell - Multi-line comment
  More text here
#>
```

## Functions and Methods

### Calling Methods

```csharp
// C# - call method
string upper = name.ToUpper();
items.Add("new item");
```

```powershell
# PowerShell - call method
$upper = $name.ToUpper()
$items.Add("new item")

# Note: Some methods return values you may want to suppress
$items.Add("new item") > $null
```

### Defining Functions

```csharp
// C# - define function
public string GetFullName(string first, string last) {
    return first + " " + last;
}

var name = GetFullName("John", "Smith");
```

```powershell
# PowerShell - define function
function Get-FullName {
    param(
        [string]$First,
        [string]$Last
    )

    return "$First $Last"
}

$name = Get-FullName -First "John" -Last "Smith"
```

## Error Handling

```csharp
// C# - try/catch
try {
    DoSomething();
}
catch(Exception ex) {
    Console.WriteLine(ex.Message);
}
finally {
    Cleanup();
}
```

```powershell
# PowerShell - try/catch
try {
    Do-Something
}
catch {
    Write-Host $_.Exception.Message
}
finally {
    Clean-up
}
```

## Performance Considerations

### Better Collections

Creating arrays with `+=` is slow. Use these instead:

```powershell
# SLOW - creates new array each time
$names = @()
$names += "Michael"
$names += "Adam"

# FAST - use ArrayList
$names = [System.Collections.ArrayList]@()
$names.Add("Michael") > $null
$names.Add("Adam") > $null

# FAST - use List<T>
$names = [System.Collections.Generic.List[string]]@()
$names.Add("Michael") > $null
$names.Add("Adam") > $null

# FAST - use HashSet for distinct values
$names = New-Object System.Collections.Generic.HashSet[string]
$names.Add("Michael") > $null
```

### Suppressing Output

Different methods have different performance:

```powershell
# SLOW - Out-Null is a cmdlet
$list.Add($item) | Out-Null

# FAST - redirect to $null
$list.Add($item) > $null
```

## Key Differences Summary

| Concept                  | C#                        | PowerShell          |
| :----------------------- | :------------------------ | :------------------ |
| **Variables**            | `var name`                | `$name`             |
| **Equals**               | `==`                      | `-eq`               |
| **Not equals**           | `!=`                      | `-ne`               |
| **And**                  | `&&`                      | `-and`              |
| **Or**                   | <code>&#124;&#124;</code> | `-or`               |
| **Not**                  | `!`                       | `-not` or `!`       |
| **Escape char**          | `\`                       | `` ` ``             |
| **String interpolation** | `$"text {var}"`           | `"text $var"`       |
| **Static access**        | `DateTime.Today`          | `[datetime]::Today` |
| **Comments**             | `//` or `/* */`           | `#` or `<# #>`      |

## Next Steps

Now that you understand the syntax:

1. **Learn commands**: [Commands and Pipelines](commands-and-pipelines.md)
2. **Understand providers**: [Providers](providers.md)
3. **Practice with examples**: [Your First Scripts](first-scripts.md)
4. **Avoid mistakes**: [Common Pitfalls](common-pitfalls.md)

As you can see, the language syntax is not all that different between C# and PowerShell. Within a few minutes you might even be able to translate code from your library classes into SPE scripts!
