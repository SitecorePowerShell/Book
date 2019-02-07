# Training

The world renowned Sitecore PowerShell Extensions module has so much to offer, but sometimes those new to the module may find it difficult to know where to start. The following book should provide you with enough information to use and be productive with SPE.

Don't worry, you will be able to use it without having to write any code.

## Videos and Blogs

We have a video series available to help walk you through the module [here](https://www.youtube.com/playlist?list=PLph7ZchYd_nCypVZSNkudGwPFRqf1na0b).

We also maintain a comprehensive list of links to [blogs and videos](https://blog.najmanowicz.com/sitecore-powershell-console/) to help you on your journey to SPE awesomeness. Happy coding!

## Language Basics

PowerShell is built on the Microsoft .Net technology; you will find that most APIs in your libraries can be accessed from within the PowerShell runtime. In this section we will see similarities between the C\# and PowerShell syntax.

### C\# to PowerShell

Use the table below to aid in translating from C\# to PowerShell. Some of the examples below are not "exact" translations, but should give you a good idea on what it would look like. For example, creating a new dynamic list in C\# is often written as a fixed dimensional array recreated with every new addition.

<table>
  <thead>
    <tr>
      <th style="text-align:left">Microsoft .Net C#</th>
      <th style="text-align:left">Windows PowerShell</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><code>// Assign data to a new variable</code><br/><code>var name = &quot;Michael&quot;;</code>
      </td>
      <td style="text-align:left"><code># Assign data to a new variable</code><br/><code>$name = &quot;Michael&quot;</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>// Perform simple math</code>  <code>var total = 1 + 1;</code>
      </td>
      <td style="text-align:left">
        <code># Perform simple math</code> 
        <br/>
        <code>$total = 1 + 1</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <code>// Create a new dynamic list of strings</code><br/>  <code>var names = new List&lt;string&gt;();</code><br/>  <code>names.Add(&quot;Michael&quot;);</code><br/> 
        <code>names.Add(&quot;Adam&quot;);</code>
      </td>
      <td style="text-align:left">
        <code># Create a new fixed list of strings</code>
        <br/>
        <code>[string[]]$names = @()</code> 
        <br/>
        <code>$names += &quot;Michael&quot;</code> 
        <br/>
        <code>$names += &quot;Adam&quot;</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
      <code>// Create a hashtable of data</code><br/>
      <code>var table = new Hashtable();</code><br/>
      <code>table[&quot;Name&quot;] = &quot;Michael&quot;;</code><br/>
      <code>table[&quot;Age&quot;] = 33;</code>
      </td>
      <td style="text-align:left">
        <code># Create a new hashtable of data</code><br/>
        <code>$table = @{}</code><br/> 
        <code>$table[&quot;Name&quot;] = &quot;Michael&quot;</code><br/>
        <code>$table[&quot;Age&quot;] = 33</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
      <code>// Ordered Dictionary</code><br/>
      <code>var od = new OrderedDictionary();</code><br/>
      <code>od.Add(&quot;z&quot;,&quot;Last Letter&quot;);</code><br/>  <code>od.Add(&quot;a&quot;,&quot;First Letter&quot;);</code>
      </td>
      <td style="text-align:left">
        <code># Ordered Dictionary </code><br/>
        <code>$od = [ordered]@{}</code><br/>
        <code>$od.Add(&quot;z&quot;,&quot;Last Letter&quot;)</code><br/>
        <code>$od.Add(&quot;a&quot;,&quot;First Letter&quot;)</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
      <code>// Check if the string is null or empty using a static method</code><br/>
      <code>if(string.IsNullOrEmpty(name)) { &#x2026; }</code>
      </td>
      <td style="text-align:left">
      <code># Check if the string is null or empty using a static method</code><br/>
      <code>if([string]::IsNullOrEmpty($name)) { &#x2026; }</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
      <code>/*</code><br/>
      <code>Create a comment block</code><br/>
      <code>*/</code>
      </td>
      <td style="text-align:left">
      <code>&lt;#</code><br/>
      <code>Create a comment block</code><br/>
      <code>#&gt;</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
      <code>// Loop through a list of strings</code><br/>
      <code>foreach(var name in names) { &#x2026; }</code>
      </td>
      <td style="text-align:left">
      <code># Loop through a list of strings</code><br/>
      <code>foreach($name in $names) { &#x2026; }</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <code>// Compare values</code> 
        <br/>
        <code>name == &quot;Michael&quot;</code><br/><code>total &lt;= 3 names.Count() &gt; 2 &amp;&amp; name[0] != &quot;Adam&quot;</code>
      </td>
      <td style="text-align:left">
        <code># Compare values</code>
        <br/>
        <code>$name -eq &quot;Michael&quot;</code>
        <br/>
        <code># case-insensitive</code>
        <br/>
        <code>$total -le 3 $names.Count -gt 2 &#x2013;and $name[0] -ne &quot;Adam&quot;</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <code>// Negate value</code><br/>
        <code>var isTrue = !false;</code>
      </td>
      <td style="text-align:left">
        <code># Negate value</code> 
        <br/>
        <code>$isTrue = !$false </code>
        <br/>
        <code>$isTrue = -not $false</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>// String interpolation</code>  <code>var message = $&quot;Hello, {name}&quot;;</code>
      </td>
      <td style="text-align:left"><code># String interpolation</code>  <code>$message = &quot;Hello, $($name)&quot;</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>// Access instance property</code>  <code>var today = DateTime.Today;</code>
      </td>
      <td style="text-align:left"><code># Access instance property</code>  <code>$today = [datetime]::Today</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>// Escape characters</code>  <code>string message = "They said to me, \"SPE is the greatest!\".";</code>
      </td>
      <td style="text-align:left"><code># Escape characters</code>  <code>$message = "They said to me, `"SPE is the greatest!`"."</code>
      </td>
    </tr>
  </tbody>
</table>As you can see in the table above, the language syntax is not all that different between C\# and PowerShell. Within a few minutes you might even be able to translate code from your library classes into SPE scripts.

### Performance Considerations

You may find yourself trying to optimize your scripts. A few things that might help include the following.

{% tabs %}
{% tab title="Collections" %}
**Example:** The following demonstrates the use of an **ArrayList**. Performs much better than purely using `@()`.

```text
# Use ArrayList to append items rather than creating new fixed dimensional arrays
$names = [System.Collections.ArrayList]@()
$names.Add("Michael") > $null
$names.Add("Adam") > $null
```

**Example:** The following demonstrates the use of **List&lt;string&gt;**. This is ideal when statically typed arrays are required.

```text
# Optionally create static static typed arrays
$names = [System.Collections.Generic.List[string]]@()
$names.Add("Michael") > $null
$names.Add("Adam") > $null
```

**Example:** The following demonstrates the use of **HashSet**. Use when a distinct list of items is needed. Performs like a **Dictionary** but without the requirement for a key.

```text
# HashSet when only values are needed
$nameLookup = New-Object System.Collections.Generic.HashSet[string]
$nameLookup.Add("Michael") > $null
```

**Example:** The following demonstrates the use of **Queue**. A Sitecore Stack Exchange answer to [find items based on a template](https://sitecore.stackexchange.com/a/15168/95) may be helpful.

```text
# Queue when ... a queue is needed
$queue = New-Object System.Collections.Queue
$queue.Enqueue("{GUID}")
$queue.Dequeue()
```
{% endtab %}

{% tab title="Measure Time" %}
**Example:** The following measures code execution time using `Measure-Command`.

```text
Measure-Command -Expression {
    Get-Item -Path "master:" > $null
} | Select-Object -ExpandProperty TotalMilliseconds
```

**Example:** The following measures code execution time using a `Stopwatch`.

```text
$watch = [System.Diagnostics.Stopwatch]::StartNew()
Get-Item -Path "master:" > $null
$watch.Stop()
$watch.ElapsedMilliseconds
```
{% endtab %}

{% tab title="Terminate Output" %}
**Example:** The following terminates the output.

```text
# Slow but functional
$builder = New-Object System.Text.StringBuilder
$builder.Append("Hello World!") | Out-Null
$builder.ToString()

# Much faster
$builder = New-Object System.Text.StringBuilder
$builder.Append("Hello World!") > $null
$builder.ToString()
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
Checklist of performance tuning ideas:

* Use dynamic arrays to void the use of `+=`
* Suppress output with `> $null` instead of `Out-Null`
* Use hashtables as lookup tables for data
{% endhint %}

## PowerShell Commands

Learning PowerShell begins with running your first command. In this section we learn about the basic command syntax, and some common ones you should learn.

### Syntax

**Example:** The following provides an example syntax for a fake command.

```text
Get-Something [[-SomeParameter] <sometype[]>] [-AnotherParameter <anothertype>] [-SomeSwitch]
```

PowerShell commands follow a Verb-Noun syntax. Notice that all properly named commands start with a verb such as Get, Set, or Remove and end with a noun such as Item, User, or Role.

{% hint style="info" %}
The noun in the command should be singular even if the command returns more than one object.
{% endhint %}

The verbs are considered “approved” if they align with those that Microsoft recommends. See the following URL [https://msdn.microsoft.com/en-us/library/ms714428\(v=vs.85\).aspx](https://msdn.microsoft.com/en-us/library/ms714428%28v=vs.85%29.aspx) for a list of approved verbs and a brief explanation on why they were chosen. They are intended to be pretty generic so they apply for multiple contexts like the filesystem, registry, and even Sitecore!

The parameters follow the command and usually require arguments. In our example above we have a parameter called **SomeParameter** followed by an argument of type **SomeType**. The final parameter **SomeSwitch** is called a switch. The switch is like a flag that enables or disables behavior for the command.

{% hint style="info" %}
The brackets surrounding the parameter and the brackets immediately following a type have different meanings. The former has to do with optional usage whereas the latter indicates the data can be an array of objects.
{% endhint %}

**Example**: The following provides possible permutations for the fake command.

```text
<#
    All of the parameters in the command are surrounded by square brackets 
    indicating they are optional.
#>
Get-Something
```

```text
<# 
    SomeParameter has double brackets around the parameter name and argument 
    indicating the name is optional and when an argument is passed the name 
    can be skipped.
#>
Get-Something "data"
```

```text
<#
    AnotherParameter has single brackets indicating that the parameter is 
    optional. If the argument is used so must the name. The same reasoning 
    can be applied to the switch.
#>
Get-Something "data","data2" -AnotherParameter 100 –SomeSwitch
```

```text
# Splat parameters to command
$props = @{
    "SomeParameter" = @("data","data2")
    "AnotherParameter" = 100
    "SomeSwitch" = $true
}
Get-Something @props
```

{% hint style="warning" %}
Allow scripts to be written with the full command and parameter names

* Avoid relying on positional or optional parameters.
* Avoid abbreviating parameter names.
* Avoid using command aliases \(e.g. dir, cd\).
{% endhint %}

Some of the most useful commands to learn can be seen in the table below. These come with vanilla PowerShell.

| Command | Description |
| :--- | :--- |
| **Get-Item** | Returns an object at the specified path. |
| **Get-ChildItem** | Returns children at the specified path. Supports recursion. |
| **Get-Help** | Returns the help documentation for the specified command or document. |
| **Get-Command** | Returns a list of commands. |
| **ForEach-Object** | Enumerates over the objects passed through the pipeline. |
| **Where-Object** | Enumerates over the objects passed through the pipeline and filters objects. |
| **Select-Object** | Returns objects from the pipeline with the specified properties and filters objects. |
| **Sort-Object** | Sorts the pipeline objects with the specified criteria; usually a property name. |
| **Get-Member** | Returns the methods and properties for the specified object. |

{% hint style="info" %}
PowerShell was designed so that after learning a few concepts you can get up and running. Once you get past the basics you should be able to understand most scripts included with SPE.
{% endhint %}

### Pipelines

PowerShell supports chaining of commands through a feature called “Pipelines” using the pipe “\|”. Similar to Sitecore in that you can short circuit the processing of objects using **Where-Object**. Let’s have a look at a few examples.

**Example:** The following queries a Sitecore item and removes it.

```text
# The remove command accepts pipeline input.
Get-Item -Path "master:\content\home\sample item" | Remove-Item

# If multiple items are passed through the pipeline each are removed individually.
$items | Remove-Item
```

PowerShell also comes with a set of useful commands for filtering and sorting. Let’s see those in action.

**Example:** The following queries a tree of Sitecore items and returns only those that meet the criteria. The item properties are reduced and then sorted.

```text
# Use variables for parameters such as paths to make scripts easier to read.
$path = "master:\content\home\"

Get-ChildItem -Path $path -Recurse | 
    Where-Object { $_.Name -like "*Sitecore*" } | 
    Select-Object -Property ID,Name,ItemPath
    Sort-Object -Property Name
```

{% hint style="info" %}
A best practice in PowerShell is to reduce the number of objects passed through the pipeline as far left as possible. While the example would work if the **Sort-Object** command came before **Where-Object**, we will see a performance improvement because the sorting has fewer objects to evaluate. Some commands such as **Get-ChildItem** support additional options for filtering which further improve performance.
{% endhint %}

**Example:** The following demonstrates how commands can be written clearly with little confusion on the intent, then how aliases and abbreviations can get in the way. Always think about the developer that comes after you to maintain the code.

```text
# Longhand - recommended
Get-Command -Name ForEach-Object –Type cmdlet | Select-Object -ExpandProperty ParameterSets

# Shorthand - not recommend
gcm -na foreach-object -ty cmdlet | select -exp parametersets
```

Windows PowerShell is bundled with a ton of documentation that could not possibly be included with this book; we can however show you how to access it.

**Example:** The following examples demonstrate ways to get help…with PowerShell.

```text
# Displays all of the about help documents.
help about_*

# Displays help documentation on the topic of Splatting.
help about_Splatting

# Displays help documentation on the specified command.
help Get-Member
```

{% hint style="info" %}
PowerShell does not include the complete help documentation by default on Windows. Run the command **Update-Help** from an elevated prompt to update the help files to the latest available version. See `help update-help` for more information on the command syntax and details of its use. All of the SPE help documentation is available regardless of running **Update-Help**.
{% endhint %}

## Providers

The provider architecture in PowerShell enables a developer to make a command like **Get-Item** interact with the filesystem files and folders, and then interact with the Sitecore CMS items.

The SPE module implements a new provider that bridges the Windows PowerShell platform with the Sitecore API. The following table demonstrates the use of **Get-Item** for a variety of providers.

| Name | Drives | Example |
| :--- | :--- | :--- |
| Alias | Alias | `Get-Item -Path alias:\dir` |
| CmsItemProvider | core, master, web | `Get-Item -Path master:\` |
| Environment | Env | `Get-Item -Path env:\HOMEPATH` |
| FileSystem | C, D, F, H | `Get-Item -Path c:\Windows` |
| Function | Function | `Get-Item -Path function:\prompt` |
| Registry | HKLM, HKCU | `Get-Item -Path hklm:\SOFTWARE` |
| Variable | Variable | `Get-Item -Path variable:\PSVersionTable` |

The default provider used by the PowerShell Console and ISE is the **CmsItemProvider** with the drive set to the **master** database.

**Example:** The following demonstrates switching between providers using the function **cd**, an alias for **Set-Location**, while in the Console.

```text
PS master:\> cd c:\
PS C:\> cd hklm:
PS HKLM:\> cd env:
PS Env:\>
```

{% hint style="warning" %}
You may have noticed that the C drive is the only path in which a backslash was used before changing drives. Leaving off the backslash will result in the path changing to C:\windows\system32\inetsrv. This similar behavior can be experienced while in the Windows PowerShell Console, where the path is changed to C:\Windows\System32.
{% endhint %}

