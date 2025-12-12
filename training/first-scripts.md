---
description: Hands-on examples to get you started with practical Sitecore automation.
---

# Your First SPE Scripts

Now that you understand the basics, let's put it all together with practical Sitecore automation tasks. These examples demonstrate real-world scenarios you'll encounter when working with SPE.

## Example 1: Find Items Ready for Review

**Goal**: Find all items under `/sitecore/content/home` that are in a specific workflow state.

```powershell
# Navigate to the home item
$homeItem = Get-Item -Path "master:\content\home"

# Get all descendants
$items = Get-ChildItem -Path $homeItem.ProviderPath -Recurse

# Filter items in "Draft" workflow state
$draftItems = $items | Where-Object {
    $_.Fields["__Workflow state"] -ne $null -and
    $_."__Workflow state" -match "{190B1C84-F1BE-47ED-AA41-F42193D9C8FC}"
}

# Display results in an interactive list
$draftItems | Show-ListView -Property Name, ItemPath, "__Updated", "__Updated by"
```

**What just happened?**

1. Retrieved the home item using the Sitecore provider → [Learn more](../working-with-items/retrieving-items.md)
2. Queried all child items recursively
3. Filtered by workflow field value → [Learn more](../working-with-items/editing-items.md)
4. Displayed results as a report → [Learn more](../appendix/common/show-listview.md)

## Example 2: Bulk Update Item Fields

**Goal**: Update a field value on multiple items.

```powershell
# Get items to update
$items = Get-ChildItem -Path "master:\content\home\articles" -Recurse |
    Where-Object { $_.TemplateName -eq "Article" }

# Update each item
foreach($item in $items) {
    # Begin editing
    $item.Editing.BeginEdit()

    # Update field
    $item["Category"] = "Updated"

    # Save changes
    $item.Editing.EndEdit()
}

Write-Host "Updated $($items.Count) items" -ForegroundColor Green
```

**Key concepts demonstrated:**

- Filtering by template → [Templates](../code-snippets/manage-templates.md)
- Proper item editing workflow → [Editing Items](../working-with-items/editing-items.md)
- Using PowerShell loops and variables

{% hint style="warning" %}
Always use `BeginEdit()` and `EndEdit()` when modifying items. Changes won't persist without this pattern.
{% endhint %}

## Example 3: Interactive User Input

**Goal**: Create a script that prompts the user for input and processes items based on their choices.

```powershell
# Prompt user for input
$rootPath = Get-Item -Path "master:\content\home"
$publishItems = $true
$props = @{
    Parameters = @(
        @{Name="rootPath"; Title="Root Path"; Tooltip="Select the starting location"},
        @{Name="templateName"; Title="Template Name";
        Source="DataSource=/sitecore/templates&DatabaseName=master&IncludeTemplatesForDisplay=Node,Folder,Template,Template Folder&IncludeTemplatesForSelection=Template";
        editor="groupeddroplist"; }
    )
    Title = "Bulk Item Processor"
    Description = "Select items to process"
    Width = 500
    Height = 300
}

$result = Read-Variable @props

if($result -ne "ok") {
    Exit
}

# Process items based on user input
$items = Get-ChildItem -Path $rootPath.ProviderPath -Recurse |
    Where-Object { $_.TemplateName -eq $templateName }

Write-Host "Found $($items.Count) items"
```

**Interactive concepts:**

- Using [Read-Variable](../appendix/common/read-variable.md) for user input
- Parameter validation
- Conditional logic based on user choices
- See [Interactive Dialogs](../interfaces/interactive-dialogs.md) for more

## Try It Yourself

Ready to experiment? Follow these steps:

### Step 1: Open the ISE

In Sitecore, go to **Desktop → Development Tools → PowerShell ISE**

### Step 2: Run Your First Command

Try these commands in the script editor:

```powershell
# Get your home item
Get-Item -Path "master:\content\home"

# List its children
Get-ChildItem -Path "master:\content\home"

# Find items by name
Get-ChildItem -Path "master:\content\home" -Recurse |
    Where-Object { $_.Name -like "*Sample*" }
```

### Step 3: Execute the Script

Click the **Execute** button (or press Ctrl+E)

### Step 4: Explore the Results

- Examine the object properties returned
- Try modifying the filters
- Experiment with different paths

{% hint style="info" %}
The ISE provides IntelliSense, syntax highlighting, and debugging capabilities. It's the best place to learn and develop SPE scripts. See [Scripting](../interfaces/scripting.md) for more details.
{% endhint %}

## Practice Exercises

Test your knowledge with these exercises:

### Exercise 1: Find Empty Items

Write a script that finds all items under `/sitecore/content` that have an empty Title field.

<details>
<summary>Solution</summary>

```powershell
Get-ChildItem -Path "master:\content" -Recurse |
    Where-Object {
        [string]::IsNullOrEmpty($_.Fields["Title"].Value)
    } |
    Show-ListView -Property Name, ItemPath, TemplateName
```

</details>

### Exercise 2: Count Items by Template

Write a script that counts how many items exist for each template under `/sitecore/content/home`.

<details>
<summary>Solution</summary>

```powershell
$items = Get-ChildItem -Path "master:\content\home" -Recurse

$templateCounts = $items | Group-Object TemplateName |
    Select-Object Name, Count |
    Sort-Object Count -Descending

$templateCounts | Show-ListView
```

</details>

### Exercise 3: Export Item Data

Export all items under a specific path to a CSV file with their Name, Path, Template, and Last Updated date.

<details>
<summary>Solution</summary>

```powershell
$items = Get-ChildItem -Path "master:\content\home" -Recurse

$export = $items | Select-Object Name,
    @{Name="Path";Expression={$_.ItemPath}},
    @{Name="Template";Expression={$_.TemplateName}},
    @{Name="Updated";Expression={$_."__Updated"}}

$export | Export-Csv -Path "$($SitecoreDataFolder)\items-export.csv" -NoTypeInformation
Write-Host "Exported $($items.Count) items"
```

</details>

## Common Patterns

Here are some frequently used patterns you'll encounter:

### Pattern: Safe Item Editing

```powershell
foreach($item in $items) {
    $item.Editing.BeginEdit()
    try {
        $item["FieldName"] = "Value"
        $item.Editing.EndEdit() | Out-Null
    }
    catch {
        $item.Editing.CancelEdit()
        Write-Error "Failed to update $($item.ItemPath): $_"
    }
}
```

### Pattern: Check if Item Exists

```powershell
$itemPath = "master:\content\home\test"
if(Test-Path $itemPath) {
    $item = Get-Item $itemPath
    # Process item
}
else {
    Write-Host "Item not found" -ForegroundColor Yellow
}
```

### Pattern: Process Items with Progress

```powershell
$items = Get-ChildItem -Path "master:\content" -Recurse
$total = $items.Count
$current = 0

foreach($item in $items) {
    $current++
    Write-Progress -Activity "Processing Items" `
                   -Status "$current of $total" `
                   -PercentComplete (($current / $total) * 100)

    # Process item here
}
```

## Next Steps

Congratulations! You've completed your first SPE scripts. Now:

1. **Avoid common mistakes**: Review [Common Pitfalls](common-pitfalls.md)
2. **Build on this knowledge**: Explore [Working with Items](../working-with-items/README.md)
3. **Create custom reports**: Learn [Authoring Reports](../modules/integration-points/reports/authoring-reports.md)
4. **Share your scripts**: Build a [Script Library](../modules/libraries-and-scripts.md)

{% hint style="success" %}
The best way to learn is by doing. Try modifying these examples for your own use cases!
{% endhint %}
