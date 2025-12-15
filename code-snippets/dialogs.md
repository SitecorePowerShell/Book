---
description: Examples for managing complex interactive dialogs.
---

# Dialogs

**Example:** Basic text controls

```powershell
<#
    .SYNOPSIS
        Demonstrates basic text input controls in Read-Variable dialogs.

    .DESCRIPTION
        Shows how to use single-line text, multi-line text, password fields,
        and placeholder text in SPE dialogs.

    .NOTES
        Documentation: https://doc.sitecorepowershell.com
#>

$dialogParams = @{
    Title = "Basic Text Controls"
    Description = "Examples of text input fields available in SPE dialogs."
    Width = 500
    Height = 400
    OkButtonName = "Submit"
    CancelButtonName = "Cancel"
    ShowHints = $true
    Parameters = @(
        @{
            Name = "singleLineText"
            Value = ""
            Title = "Single Line Text"
            Tooltip = "A simple single-line text input"
            Placeholder = "Enter text here..."
        },
        @{
            Name = "multiLineText"
            Value = ""
            Title = "Multi-Line Text"
            Lines = 3
            Tooltip = "A multi-line text area for longer content"
            Placeholder = "Enter multiple lines of text..."
        },
        @{
            Name = "passwordField"
            Value = ""
            Title = "Password"
            Editor = "password"
            Tooltip = "Password input - characters are masked"
            Placeholder = "Enter password..."
        }
    )
}

$result = Read-Variable @dialogParams

if ($result -eq "ok") {
    Write-Host "Single Line Text: $singleLineText" -ForegroundColor Green
    Write-Host "Multi-Line Text: $multiLineText" -ForegroundColor Green
    Write-Host "Password: $passwordField" -ForegroundColor Green
}
```

**Example:** Number controls

```powershell
<#
    .SYNOPSIS
        Demonstrates number input controls in Read-Variable dialogs.

    .DESCRIPTION
        Shows how to use integer and floating-point number inputs in SPE dialogs.

    .NOTES
        Documentation: https://doc.sitecorepowershell.com
#>

$dialogParams = @{
    Title = "Number Input Controls"
    Description = "Examples of numeric input fields."
    Width = 400
    Height = 300
    OkButtonName = "Submit"
    CancelButtonName = "Cancel"
    ShowHints = $true
    Parameters = @(
        @{
            Name = "integerValue"
            Value = 100
            Editor = "number"
            Title = "Integer Value"
            Tooltip = "Enter a whole number"
            Columns = 6
        },
        @{
            Name = "floatValue"
            Value = 1.5
            Editor = "number"
            Title = "Decimal Value"
            Tooltip = "Enter a decimal number"
            Columns = 6
        }
    )
}

$result = Read-Variable @dialogParams

if ($result -eq "ok") {
    Write-Host "Integer Value: $integerValue" -ForegroundColor Green
    Write-Host "Float Value: $floatValue" -ForegroundColor Green

    # Demonstrate the returned types
    Write-Host "`nReturned Types:" -ForegroundColor Cyan
    Write-Host "Integer Type: $($integerValue.GetType().Name)"
    Write-Host "Float Type: $($floatValue.GetType().Name)"
}
```

**Example:** Checkbox control

```powershell
<#
    .SYNOPSIS
        Demonstrates checkbox control in Read-Variable dialogs.

    .DESCRIPTION
        Shows how to use checkbox inputs that return boolean values.

    .NOTES
        Documentation: https://doc.sitecorepowershell.com
#>

# Initialize default value
$enableFeature = $true

$dialogParams = @{
    Title = "Checkbox Control"
    Description = "Example of checkbox input that returns a boolean value."
    Width = 400
    Height = 250
    OkButtonName = "Submit"
    CancelButtonName = "Cancel"
    ShowHints = $true
    Parameters = @(
        @{
            Name = "enableFeature"
            Value = $true
            Title = "Enable Feature"
            Tooltip = "Check to enable, uncheck to disable"
            Editor = "checkbox"
        },
        @{
            Name = "includeSubitems"
            Value = $false
            Title = "Include Subitems"
            Tooltip = "Process child items as well"
            Editor = "checkbox"
        },
        @{
            Name = "publishAfterSave"
            Value = $false
            Title = "Publish After Save"
            Tooltip = "Automatically publish changes"
            Editor = "checkbox"
        }
    )
}

$result = Read-Variable @dialogParams

if ($result -eq "ok") {
    Write-Host "Enable Feature: $enableFeature" -ForegroundColor Green
    Write-Host "Include Subitems: $includeSubitems" -ForegroundColor Green
    Write-Host "Publish After Save: $publishAfterSave" -ForegroundColor Green

    # Example conditional logic based on checkbox values
    if ($enableFeature) {
        Write-Host "`nFeature is enabled!" -ForegroundColor Cyan
    }
}
```

**Example:** Dropdown combo controls

```powershell
<#
    .SYNOPSIS
        Demonstrates dropdown (combo) controls in Read-Variable dialogs.

    .DESCRIPTION
        Shows how to create dropdown lists with predefined options and tooltips.

    .NOTES
        Documentation: https://doc.sitecorepowershell.com
#>

# Define options as an ordered hashtable
# Format: @{ "Display Text" = "Value" }
$priorityOptions = [ordered]@{
    "Low" = "1"
    "Medium" = "2"
    "High" = "3"
    "Critical" = "4"
}

# Options with tooltips
$actionOptions = [ordered]@{
    "Create" = "create"
    "Update" = "update"
    "Delete" = "delete"
    "Archive" = "archive"
}

$actionTooltips = [ordered]@{
    "create" = "Create a new item in Sitecore"
    "update" = "Modify an existing item"
    "delete" = "Remove the item permanently"
    "archive" = "Move the item to archive"
}

$dialogParams = @{
    Title = "Dropdown Controls"
    Description = "Examples of combo/dropdown selection controls."
    Width = 450
    Height = 350
    OkButtonName = "Submit"
    CancelButtonName = "Cancel"
    ShowHints = $true
    Parameters = @(
        @{
            Name = "selectedPriority"
            Value = "2"
            Title = "Priority Level"
            Options = $priorityOptions
            Tooltip = "Select the priority for this task"
            Editor = "combo"
        },
        @{
            Name = "selectedAction"
            Value = "update"
            Title = "Action Type"
            Options = $actionOptions
            OptionTooltips = $actionTooltips
            Tooltip = "Choose the action to perform"
            Editor = "combo"
        }
    )
}

$result = Read-Variable @dialogParams

if ($result -eq "ok") {
    Write-Host "Selected Priority: $selectedPriority" -ForegroundColor Green
    Write-Host "Selected Action: $selectedAction" -ForegroundColor Green
}
```

**Example:** Checklist control

```powershell
<#
    .SYNOPSIS
        Demonstrates checklist control for multiple selections in Read-Variable dialogs.

    .DESCRIPTION
        Shows how to create a checklist that allows users to select multiple options.
        The checklist uses bitmask values for selections.

    .NOTES
        Documentation: https://doc.sitecorepowershell.com
#>

# Define options with bitmask values
# Each value should be a power of 2 for proper bitmask operations
$daysOfWeek = [ordered]@{
    "Monday" = 1
    "Tuesday" = 2
    "Wednesday" = 4
    "Thursday" = 8
    "Friday" = 16
    "Saturday" = 32
    "Sunday" = 64
}

# Pre-select some items by providing an array of values
$selectedDays = @(1, 4, 16)  # Monday, Wednesday, Friday

$languageOptions = [ordered]@{
    "English" = 1
    "German" = 2
    "French" = 4
    "Spanish" = 8
    "Japanese" = 16
}

$selectedLanguages = @(1)  # English pre-selected

$dialogParams = @{
    Title = "Checklist Controls"
    Description = "Select multiple options using checklists."
    Width = 450
    Height = 450
    OkButtonName = "Submit"
    CancelButtonName = "Cancel"
    ShowHints = $true
    Parameters = @(
        @{
            Name = "selectedDays"
            Title = "Days of the Week"
            Options = $daysOfWeek
            Tooltip = "Select which days to run the scheduled task"
            Editor = "checklist"
        },
        @{
            Name = "selectedLanguages"
            Title = "Target Languages"
            Options = $languageOptions
            Tooltip = "Select languages for content translation"
            Editor = "checklist"
        }
    )
}

$result = Read-Variable @dialogParams

if ($result -eq "ok") {
    Write-Host "Selected Days (values): $($selectedDays -join ', ')" -ForegroundColor Green
    Write-Host "Selected Languages (values): $($selectedLanguages -join ', ')" -ForegroundColor Green

    # Map values back to display names
    Write-Host "`nSelected Day Names:" -ForegroundColor Cyan
    foreach ($day in $daysOfWeek.GetEnumerator()) {
        if ($selectedDays -contains $day.Value) {
            Write-Host "  - $($day.Key)"
        }
    }
}
```

**Example:** Radio button control

```powershell
<#
    .SYNOPSIS
        Demonstrates radio button control for single selection in Read-Variable dialogs.

    .DESCRIPTION
        Shows how to create radio button groups that allow users to select exactly one option.

    .NOTES
        Documentation: https://doc.sitecorepowershell.com
#>

# Define options for radio buttons
$publishMode = [ordered]@{
    "Smart Publish" = 1
    "Incremental Publish" = 2
    "Full Republish" = 3
}

$targetDatabase = [ordered]@{
    "Web" = "web"
    "Preview" = "preview"
    "Staging" = "staging"
}

# Pre-select an option
$selectedPublishMode = 1
$selectedDatabase = "web"

$dialogParams = @{
    Title = "Radio Button Controls"
    Description = "Select exactly one option from each group."
    Width = 450
    Height = 400
    OkButtonName = "Submit"
    CancelButtonName = "Cancel"
    ShowHints = $true
    Parameters = @(
        @{
            Name = "selectedPublishMode"
            Title = "Publish Mode"
            Options = $publishMode
            Tooltip = "Choose how content should be published"
            Editor = "radio"
        },
        @{
            Name = "selectedDatabase"
            Title = "Target Database"
            Options = $targetDatabase
            Tooltip = "Select the destination database"
            Editor = "radio"
        }
    )
}

$result = Read-Variable @dialogParams

if ($result -eq "ok") {
    Write-Host "Selected Publish Mode: $selectedPublishMode" -ForegroundColor Green
    Write-Host "Selected Database: $selectedDatabase" -ForegroundColor Green

    # Use the selection in logic
    switch ($selectedPublishMode) {
        1 { Write-Host "`nPerforming Smart Publish..." -ForegroundColor Cyan }
        2 { Write-Host "`nPerforming Incremental Publish..." -ForegroundColor Cyan }
        3 { Write-Host "`nPerforming Full Republish..." -ForegroundColor Cyan }
    }
}
```

**Example:** Date Time control

```powershell
<#
    .SYNOPSIS
        Demonstrates date and time picker controls in Read-Variable dialogs.
    
    .DESCRIPTION
        Shows how to use date-only and date-time picker controls.
        Both return System.DateTime objects.
    
    .NOTES
        Documentation: https://doc.sitecorepowershell.com
#>

# Set default values
$startDateTime = [System.DateTime]::Now.AddDays(-7)
$endDateTime = [System.DateTime]::Now
$publishDate = [System.DateTime]::Now.AddDays(1)

$dialogParams = @{
    Title = "Date and Time Controls"
    Description = "Select dates and times for your operation."
    Width = 500
    Height = 400
    OkButtonName = "Submit"
    CancelButtonName = "Cancel"
    ShowHints = $true
    Parameters = @(
        @{
            Name = "startDateTime"
            Value = [System.DateTime]::Now.AddDays(-7)
            Title = "Start Date/Time"
            Tooltip = "Select the starting date and time"
            Editor = "date time"
        },
        @{
            Name = "endDateTime"
            Value = [System.DateTime]::Now
            Title = "End Date/Time"
            Tooltip = "Select the ending date and time"
            Editor = "date time"
        },
        @{
            Name = "publishDate"
            Value = [System.DateTime]::Now.AddDays(1)
            Title = "Publish Date (Date Only)"
            Tooltip = "Select the date for publishing"
            Editor = "date"
        }
    )
}

$result = Read-Variable @dialogParams

if ($result -eq "ok") {
    Write-Host "Date/Time Controls return System.DateTime objects:" -ForegroundColor Cyan
    Write-Host ""
    
    Write-Host "Start DateTime: $startDateTime" -ForegroundColor Green
    Write-Host "  Type: $($startDateTime.GetType().FullName)"
    Write-Host ""
    
    Write-Host "End DateTime: $endDateTime" -ForegroundColor Green
    Write-Host "  Type: $($endDateTime.GetType().FullName)"
    Write-Host ""
    
    Write-Host "Publish Date: $publishDate" -ForegroundColor Green
    Write-Host "  Type: $($publishDate.GetType().FullName)"
    Write-Host ""
    
    # Calculate duration
    $duration = $endDateTime - $startDateTime
    Write-Host "Duration: $($duration.Days) days, $($duration.Hours) hours" -ForegroundColor Yellow
}
```

**Example:** Single item picker with a tree

```powershell
<#
    .SYNOPSIS
        Demonstrates single item picker control in Read-Variable dialogs.
    
    .DESCRIPTION
        Shows how to use the item picker to select a single Sitecore item.
        Returns a Sitecore Item object.
    
    .NOTES
        Documentation: https://doc.sitecorepowershell.com
#>

# Initialize item variables with default Item objects
# Read-Variable requires item variables to be pre-initialized
$selectedItem = Get-Item -Path "master:\content\Home"
$templateItem = Get-Item -Path "master:\templates\System\Templates\Template"

$dialogParams = @{
    Title = "Single Item Picker"
    Description = "Select a single item from the content tree."
    Width = 500
    Height = 350
    OkButtonName = "Select"
    CancelButtonName = "Cancel"
    ShowHints = $true
    Parameters = @(
        @{
            Name = "selectedItem"
            Title = "Select Content Item"
            Root = "/sitecore/content/"
            Tooltip = "Browse and select an item from the content tree"
        },
        @{
            Name = "templateItem"
            Title = "Select Template"
            Root = "/sitecore/templates/"
            Tooltip = "Browse and select a template"
        }
    )
}

$result = Read-Variable @dialogParams

if ($result -eq "ok") {
    Write-Host "Item Picker returns Sitecore Item objects:" -ForegroundColor Cyan
    Write-Host ""
    
    if ($selectedItem) {
        Write-Host "Selected Content Item:" -ForegroundColor Green
        Write-Host "  Name: $($selectedItem.Name)"
        Write-Host "  Path: $($selectedItem.ItemPath)"
        Write-Host "  ID: $($selectedItem.ID)"
        Write-Host "  Template: $($selectedItem.TemplateName)"
    } else {
        Write-Host "No content item selected" -ForegroundColor Yellow
    }
    
    Write-Host ""
    
    if ($templateItem) {
        Write-Host "Selected Template:" -ForegroundColor Green
        Write-Host "  Name: $($templateItem.Name)"
        Write-Host "  Path: $($templateItem.ItemPath)"
        Write-Host "  ID: $($templateItem.ID)"
    } else {
        Write-Host "No template selected" -ForegroundColor Yellow
    }
}
```

**Example:** Treelist control

```powershell
<#
    .SYNOPSIS
        Demonstrates treelist control for multiple item selection in Read-Variable dialogs.
    
    .DESCRIPTION
        Shows how to use treelist to select multiple items with a tree-based interface.
        Returns an array of Sitecore Item objects.
    
    .NOTES
        Documentation: https://doc.sitecorepowershell.com
#>

# Initialize with an array of Item objects (can be empty array or pre-selected items)
$selectedTemplates = @(Get-Item -Path "master:\templates\System\Templates\Template")

$dialogParams = @{
    Title = "Treelist Control"
    Description = "Select multiple items using the treelist interface."
    Width = 600
    Height = 500
    OkButtonName = "Select"
    CancelButtonName = "Cancel"
    ShowHints = $true
    Parameters = @(
        @{
            Name = "selectedTemplates"
            Title = "Select Templates"
            Source = "DataSource=/sitecore/templates&DatabaseName=master&IncludeTemplatesForDisplay=Node,Folder,Template,Template Folder&IncludeTemplatesForSelection=Template"
            Editor = "treelist"
            Tooltip = "Select one or more templates from the tree"
        }
    )
}

$result = Read-Variable @dialogParams

if ($result -eq "ok") {
    Write-Host "Treelist returns an array of Sitecore Item objects:" -ForegroundColor Cyan
    Write-Host ""
    
    if ($selectedTemplates -and $selectedTemplates.Count -gt 0) {
        Write-Host "Selected $($selectedTemplates.Count) template(s):" -ForegroundColor Green
        Write-Host ""
        
        foreach ($template in $selectedTemplates) {
            Write-Host "  - $($template.Name)" -ForegroundColor White
            Write-Host "    Path: $($template.ItemPath)"
            Write-Host "    ID: $($template.ID)"
            Write-Host ""
        }
    } else {
        Write-Host "No templates selected" -ForegroundColor Yellow
    }
}

<#
    TREELIST SOURCE PARAMETERS:
    
    The Source parameter uses a query string format with these options:
    
    - DataSource: Root path for the tree (e.g., /sitecore/templates)
    - DatabaseName: Target database (master, web, core)
    - IncludeTemplatesForDisplay: Templates to show in tree (comma-separated)
    - IncludeTemplatesForSelection: Templates that can be selected (comma-separated)
    - ExcludeTemplatesForDisplay: Templates to hide
    - ExcludeTemplatesForSelection: Templates that cannot be selected
    
    Example configurations:
    
    # Select pages under content
    Source = "DataSource=/sitecore/content&DatabaseName=master&IncludeTemplatesForSelection=Page,Article"
    
    # Select media items
    Source = "DataSource=/sitecore/media library&DatabaseName=master&IncludeTemplatesForSelection=Image,Jpeg,Png"
#>
```

**Example:** Multilist controls

```powershell
<#
    .SYNOPSIS
        Demonstrates multilist control for multiple item selection in Read-Variable dialogs.
    
    .DESCRIPTION
        Shows how to use multilist and multilist search controls.
        Both return arrays of Sitecore Item objects.
    
    .NOTES
        Documentation: https://doc.sitecorepowershell.com
#>

# Initialize item variable - required for item picker
$selectedItems = @(Get-Item -Path "master:\templates\System\Templates\Template")
$searchableItems = @(Get-Item -Path "master:\templates\System\Templates\Standard template")

$dialogParams = @{
    Title = "Multilist Controls"
    Description = "Select multiple items using multilist interfaces."
    Width = 650
    Height = 600
    OkButtonName = "Select"
    CancelButtonName = "Cancel"
    ShowHints = $true
    Parameters = @(
        @{
            Name = "selectedItems"
            Title = "Multilist (Standard)"
            Source = "DataSource=/sitecore/templates/System&DatabaseName=master&IncludeTemplatesForDisplay=Template Folder,Template&IncludeTemplatesForSelection=Template"
            Editor = "multilist"
            Height = "200px"
            Tooltip = "Select items by moving them between lists"
        },
        @{
            Name = "searchableItems"
            Title = "Multilist with Search"
            Source = "DataSource=/sitecore/templates&DatabaseName=master&IncludeTemplatesForDisplay=Template Folder,Template&IncludeTemplatesForSelection=Template"
            Editor = "multilist search"
            Height = "200px"
            Tooltip = "Search and select items - great for large lists"
        }
    )
}

$result = Read-Variable @dialogParams

if ($result -eq "ok") {
    Write-Host "Multilist controls return arrays of Sitecore Item objects:" -ForegroundColor Cyan
    Write-Host ""
    
    Write-Host "Standard Multilist Selection:" -ForegroundColor Green
    if ($selectedItems -and $selectedItems.Count -gt 0) {
        foreach ($item in $selectedItems) {
            Write-Host "  - $($item.Name) [$($item.ID)]"
        }
    } else {
        Write-Host "  (none selected)" -ForegroundColor Yellow
    }
    
    Write-Host ""
    Write-Host "Multilist Search Selection:" -ForegroundColor Green
    if ($searchableItems -and $searchableItems.Count -gt 0) {
        foreach ($item in $searchableItems) {
            Write-Host "  - $($item.Name) [$($item.ID)]"
        }
    } else {
        Write-Host "  (none selected)" -ForegroundColor Yellow
    }
}

<#
    MULTILIST vs MULTILIST SEARCH:
    
    Standard Multilist:
    - Shows all available items in a scrollable list
    - Best for smaller datasets (< 100 items)
    - Drag-and-drop interface
    
    Multilist Search:
    - Includes a search box to filter items
    - Best for larger datasets where browsing isn't practical
    - Same selection behavior as standard multilist
    
    Both use the same Source parameter format as treelist.
#>
```

**Example::** Droplist contol

```powershell
<#
    .SYNOPSIS
        Demonstrates droplist control for single item selection in Read-Variable dialogs.
    
    .DESCRIPTION
        Shows how to use droplist to select a single item from a flat list.
        Returns a Sitecore Item object.
    
    .NOTES
        Documentation: https://doc.sitecorepowershell.com
#>

# Initialize item variable - required for item picker
$selectedTemplate = Get-Item -Path "master:\templates\sample\sample item"

$dialogParams = @{
    Title = "Droplist Control"
    Description = "Select a single item from a dropdown list."
    Width = 500
    Height = 300
    OkButtonName = "Select"
    CancelButtonName = "Cancel"
    ShowHints = $true
    Parameters = @(
        @{
            Name = "selectedTemplate"
            Title = "Select a Template"
            Source = "DataSource=/sitecore/templates/sample&DatabaseName=master&IncludeTemplatesForDisplay=Template Folder,Template&IncludeTemplatesForSelection=Template"
            Editor = "droplist"
            Tooltip = "Choose a single template from the dropdown"
        }
    )
}

$result = Read-Variable @dialogParams

if ($result -eq "ok") {
    Write-Host "Droplist returns a single Sitecore Item object:" -ForegroundColor Cyan
    Write-Host ""
    
    if ($selectedTemplate) {
        Write-Host "Selected Template:" -ForegroundColor Green
        Write-Host "  Name: $($selectedTemplate.Name)"
        Write-Host "  Path: $($selectedTemplate.ItemPath)"
        Write-Host "  ID: $($selectedTemplate.ID)"
        Write-Host "  Type: $($selectedTemplate.GetType().FullName)"
    } else {
        Write-Host "No template selected" -ForegroundColor Yellow
    }
}

<#
    DROPLIST vs COMBO:
    
    Droplist:
    - Displays Sitecore items from a data source
    - Returns a Sitecore Item object
    - Uses Source parameter with DataSource path
    
    Combo:
    - Displays key-value pairs defined in script
    - Returns the value (usually a string or number)
    - Uses Options parameter with hashtable
    
    Use Droplist when you need to select from Sitecore items.
    Use Combo when you need predefined options.
#>
```

**Example:** Grouped dropdown controls

```powershell
<#
    .SYNOPSIS
        Demonstrates grouped dropdown controls in Read-Variable dialogs.
    
    .DESCRIPTION
        Shows how to use groupeddroplink and groupeddroplist controls.
        These display items grouped by their parent folders.
        - groupeddroplink: Returns the selected Item object
        - groupeddroplist: Returns the item's string value (path or ID)
    
    .NOTES
        Documentation: https://doc.sitecorepowershell.com
#>

$dialogParams = @{
    Title = "Grouped Dropdown Controls"
    Description = "Select items from grouped dropdown lists."
    Width = 550
    Height = 350
    OkButtonName = "Select"
    CancelButtonName = "Cancel"
    ShowHints = $true
    Parameters = @(
        @{
            Name = "selectedGroupedDroplink"
            Title = "Grouped Droplink (Returns Item)"
            Source = "DataSource=/sitecore/templates&DatabaseName=master&IncludeTemplatesForDisplay=Node,Folder,Template,Template Folder&IncludeTemplatesForSelection=Template"
            Editor = "groupeddroplink"
            Tooltip = "Items grouped by folder - returns Item object"
        },
        @{
            Name = "selectedGroupedDroplist"
            Title = "Grouped Droplist (Returns String)"
            Source = "DataSource=/sitecore/templates&DatabaseName=master&IncludeTemplatesForDisplay=Node,Folder,Template,Template Folder&IncludeTemplatesForSelection=Template"
            Editor = "groupeddroplist"
            Tooltip = "Items grouped by folder - returns string value"
        }
    )
}

$result = Read-Variable @dialogParams

if ($result -eq "ok") {
    Write-Host "Grouped Dropdown Controls:" -ForegroundColor Cyan
    Write-Host ""
    
    Write-Host "Grouped Droplink (returns Item):" -ForegroundColor Green
    if ($selectedGroupedDroplink) {
        Write-Host "  Name: $($selectedGroupedDroplink.Name)"
        Write-Host "  Path: $($selectedGroupedDroplink.ItemPath)"
        Write-Host "  Type: $($selectedGroupedDroplink.GetType().Name)"
    } else {
        Write-Host "  (none selected)" -ForegroundColor Yellow
    }
    
    Write-Host ""
    Write-Host "Grouped Droplist (returns String):" -ForegroundColor Green
    if ($selectedGroupedDroplist) {
        Write-Host "  Value: $selectedGroupedDroplist"
        Write-Host "  Type: $($selectedGroupedDroplist.GetType().Name)"
    } else {
        Write-Host "  (none selected)" -ForegroundColor Yellow
    }
}

<#
    GROUPED DROPDOWNS:
    
    Both groupeddroplink and groupeddroplist display items organized
    by their parent folders, making it easier to navigate large item sets.
    
    Key Difference:
    - groupeddroplink: Returns the actual Sitecore Item object
    - groupeddroplist: Returns a string representation
    
    Use groupeddroplink when you need to work with the Item object.
    Use groupeddroplist when you only need the item's path or ID.
#>
```

**Example:** User role pickers

```powershell
<#
    .SYNOPSIS
        Demonstrates user and role picker controls in Read-Variable dialogs.
    
    .DESCRIPTION
        Shows how to use user, role, and combined user/role selection controls.
        These are essential for security and permission-related scripts.
    
    .NOTES
        Documentation: https://doc.sitecorepowershell.com
#>

$dialogParams = @{
    Title = "User and Role Pickers"
    Description = "Select users and roles for permission management."
    Width = 550
    Height = 450
    OkButtonName = "Select"
    CancelButtonName = "Cancel"
    ShowHints = $true
    Parameters = @(
        @{
            Name = "selectedUser"
            Title = "Select User(s)"
            Editor = "user multiple"
            Tooltip = "Select one or more users"
        },
        @{
            Name = "selectedRole"
            Title = "Select Role(s)"
            Editor = "role multiple"
            Domain = "sitecore"
            Tooltip = "Select one or more roles from the sitecore domain"
        },
        @{
            Name = "selectedUserOrRole"
            Title = "Select User(s) or Role(s)"
            Editor = "user role multiple"
            Domain = "sitecore"
            Tooltip = "Select any combination of users and roles"
        }
    )
}

$result = Read-Variable @dialogParams

if ($result -eq "ok") {
    Write-Host "User and Role Pickers return string values:" -ForegroundColor Cyan
    Write-Host ""
    
    Write-Host "Selected Users:" -ForegroundColor Green
    if ($selectedUser) {
        $selectedUser | ForEach-Object { Write-Host "  - $_" }
    } else {
        Write-Host "  (none selected)" -ForegroundColor Yellow
    }
    
    Write-Host ""
    Write-Host "Selected Roles:" -ForegroundColor Green
    if ($selectedRole) {
        $selectedRole | ForEach-Object { Write-Host "  - $_" }
    } else {
        Write-Host "  (none selected)" -ForegroundColor Yellow
    }
    
    Write-Host ""
    Write-Host "Selected Users or Roles:" -ForegroundColor Green
    if ($selectedUserOrRole) {
        $selectedUserOrRole | ForEach-Object { Write-Host "  - $_" }
    } else {
        Write-Host "  (none selected)" -ForegroundColor Yellow
    }
}

<#
    USER AND ROLE EDITOR VARIANTS:
    
    Single Selection:
    - "user"      : Select one user
    - "role"      : Select one role
    - "user role" : Select one user or role
    
    Multiple Selection:
    - "user multiple"      : Select multiple users
    - "role multiple"      : Select multiple roles
    - "user role multiple" : Select multiple users and/or roles
    
    Domain Parameter:
    The Domain parameter filters roles to a specific security domain.
    Common domains: "sitecore", "extranet", custom domains
    
    Return Values:
    - Single selection returns a string
    - Multiple selection returns an array of strings
    - Format: "domain\username" or "domain\rolename"
#>
```

**Example:** Rule editor controls

```powershell
<#
    .SYNOPSIS
        Demonstrates rule editor controls in Read-Variable dialogs.
    
    .DESCRIPTION
        Shows how to use rule and rule action editors for creating 
        Sitecore rules with conditions and actions.
    
    .NOTES
        Documentation: https://doc.sitecorepowershell.com
#>

# You can pre-populate with an existing rule
# Get a sample rule from an existing item
$existingRule = $null
$ruleItem = Get-Item -Path "master:" -ID "{D00BD134-EB15-41A7-BEF1-E6455C6BC9AC}" -ErrorAction SilentlyContinue
if ($ruleItem) {
    $existingRule = $ruleItem | Select-Object -ExpandProperty ShowRule -ErrorAction SilentlyContinue
}

$dialogParams = @{
    Title = "Rule Editor Controls"
    Description = "Create rules with conditions and optional actions."
    Width = 700
    Height = 500
    OkButtonName = "Save Rules"
    CancelButtonName = "Cancel"
    ShowHints = $true
    Parameters = @(
        @{
            Name = "conditionRule"
            Title = "Condition Rule"
            Editor = "rule"
            Tooltip = "Define conditions only (no actions)"
        },
        @{
            Name = "actionRule"
            Title = "Rule with Actions"
            Editor = "rule action"
            Tooltip = "Define conditions and actions to execute"
        }
    )
}

$result = Read-Variable @dialogParams

if ($result -eq "ok") {
    Write-Host "Rule Editors return XML string values:" -ForegroundColor Cyan
    Write-Host ""
    
    Write-Host "Condition Rule:" -ForegroundColor Green
    if ($conditionRule) {
        Write-Host "  Length: $($conditionRule.Length) characters"
        Write-Host "  Preview: $($conditionRule.Substring(0, [Math]::Min(100, $conditionRule.Length)))..."
    } else {
        Write-Host "  (no rule defined)" -ForegroundColor Yellow
    }
    
    Write-Host ""
    Write-Host "Rule with Actions:" -ForegroundColor Green
    if ($actionRule) {
        Write-Host "  Length: $($actionRule.Length) characters"
        Write-Host "  Preview: $($actionRule.Substring(0, [Math]::Min(100, $actionRule.Length)))..."
    } else {
        Write-Host "  (no rule defined)" -ForegroundColor Yellow
    }
}

<#
    RULE EDITOR TYPES:
    
    "rule" Editor:
    - Conditions only (WHERE clauses)
    - Use for filtering, visibility rules, validation
    - Example: Show field when template is X
    
    "rule action" Editor:
    - Conditions AND actions
    - Use for automation, workflows, scheduled tasks
    - Example: When item is created, set default values
    
    Return Value:
    Both editors return an XML string representing the rule definition.
    This XML can be stored in a rule field or parsed for processing.
    
    Common Use Cases:
    - Content validation rules
    - Conditional rendering rules
    - Workflow state transitions
    - Scheduled task filters
    - Personalization conditions
#>
```

**Example:** Info display controls

```powershell
<#
    .SYNOPSIS
        Demonstrates information display controls in Read-Variable dialogs.
    
    .DESCRIPTION
        Shows how to use info and marquee controls to display 
        read-only information and messages to users.
    
    .NOTES
        Documentation: https://doc.sitecorepowershell.com
#>

$dialogParams = @{
    Title = "Information Display Controls"
    Description = "Display important information to users."
    Width = 550
    Height = 400
    OkButtonName = "Continue"
    CancelButtonName = "Cancel"
    ShowHints = $true
    Parameters = @(
        @{
            Name = "marqueeMessage"
            Title = "Marquee - Scrolling Message"
            Value = "This is an important scrolling message that catches attention!"
            Editor = "marquee"
        },
        @{
            Name = "infoMessage"
            Title = "Info - Static Information"
            Value = "This is static informational text that provides context or instructions to the user. It does not scroll and is easier to read for longer messages."
            Editor = "info"
        },
        @{
            Name = "userInput"
            Title = "Your Response"
            Value = ""
            Tooltip = "Enter your response after reading the information above"
        }
    )
}

$result = Read-Variable @dialogParams

if ($result -eq "ok") {
    Write-Host "User acknowledged the information and provided:" -ForegroundColor Cyan
    Write-Host "Response: $userInput" -ForegroundColor Green
}

<#
    DISPLAY CONTROLS (Read-Only):
    
    "marquee" Editor:
    - Displays scrolling text
    - Good for alerts or important notices
    - Catches user attention with animation
    
    "info" Editor:
    - Displays static text
    - Good for instructions, context, or longer messages
    - No animation, easier to read
    
    Common Use Cases:
    - Display warnings before destructive operations
    - Show instructions for complex forms
    - Provide context about selected items
    - Display calculated values or status information
    
    Note: These are display-only controls. The Value is shown
    but not returned or modified by user interaction.
#>
```

**Example:** Variable binding

```powershell
<#
    .SYNOPSIS
        Demonstrates variable binding in Read-Variable dialogs.
    
    .DESCRIPTION
        Shows how to bind existing PowerShell variables to dialog controls
        using the Variable parameter instead of Name/Value.
    
    .NOTES
        Documentation: https://doc.sitecorepowershell.com
#>

# Get an existing item to use as a variable
$parentItem = Get-Item -Path "master:\content\Home" -ErrorAction SilentlyContinue
if (-not $parentItem) {
    $parentItem = Get-Item -Path "master:\content" -ErrorAction SilentlyContinue
}

# Define other variables
$searchQuery = "sample query"
$maxResults = 50

$dialogParams = @{
    Title = "Variable Binding Example"
    Description = "Bind existing variables directly to dialog controls."
    Width = 500
    Height = 400
    OkButtonName = "Process"
    CancelButtonName = "Cancel"
    ShowHints = $true
    Parameters = @(
        @{
            # Use Variable instead of Name to bind directly to existing variable
            Variable = Get-Variable "parentItem"
            Title = "Parent Item (Variable Binding)"
            Tooltip = "This control is bound to the `$parentItem variable"
        },
        @{
            Variable = Get-Variable "searchQuery"
            Title = "Search Query (Variable Binding)"
            Tooltip = "Bound to `$searchQuery variable"
        },
        @{
            Variable = Get-Variable "maxResults"
            Title = "Max Results (Variable Binding)"
            Editor = "number"
            Tooltip = "Bound to `$maxResults variable"
        }
    )
}

$result = Read-Variable @dialogParams

if ($result -eq "ok") {
    Write-Host "Variable Binding Results:" -ForegroundColor Cyan
    Write-Host ""
    
    Write-Host "Parent Item:" -ForegroundColor Green
    if ($parentItem) {
        Write-Host "  Name: $($parentItem.Name)"
        Write-Host "  Path: $($parentItem.ItemPath)"
    }
    
    Write-Host ""
    Write-Host "Search Query: $searchQuery" -ForegroundColor Green
    Write-Host "Max Results: $maxResults" -ForegroundColor Green
}

<#
    VARIABLE BINDING vs NAME/VALUE:
    
    Name/Value Approach:
    - Creates a new variable with the specified Name
    - Initial value set with Value parameter
    @{ Name = "myVar"; Value = "initial" }
    
    Variable Binding:
    - Binds directly to an existing PowerShell variable
    - Use Get-Variable to reference the variable
    - Variable is updated in-place when dialog closes
    @{ Variable = Get-Variable "existingVar" }
    
    When to Use Variable Binding:
    - When you already have variables with values from previous operations
    - When you want to modify existing context variables
    - When working with items from the current context (like $item)
    
    Benefits:
    - Less boilerplate code
    - Variables retain their type information
    - Direct modification of existing state
#>
```

**Example:** Tabbed dialog

```powershell
<#
    .SYNOPSIS
        Demonstrates tabbed dialogs in Read-Variable.
    
    .DESCRIPTION
        Shows how to organize controls into multiple tabs for complex dialogs.
        Use the Tab parameter to assign controls to specific tabs.
    
    .NOTES
        Documentation: https://doc.sitecorepowershell.com
#>

# Initialize item variable - required for item picker
$template = Get-Item -Path "master:\templates\sample\sample item"

$dialogParams = @{
    Title = "Tabbed Dialog Example"
    Description = "Controls organized into logical tabs."
    Width = 600
    Height = 500
    OkButtonName = "Save"
    CancelButtonName = "Cancel"
    ShowHints = $true
    Parameters = @(
        # General Tab
        @{
            Name = "itemName"
            Title = "Item Name"
            Value = ""
            Tooltip = "Enter the name for the new item"
            Tab = "General"
            Placeholder = "Enter name..."
        },
        @{
            Name = "itemTitle"
            Title = "Display Title"
            Value = ""
            Tooltip = "The title shown to users"
            Tab = "General"
            Placeholder = "Enter title..."
        },
        @{
            Name = "description"
            Title = "Description"
            Value = ""
            Lines = 3
            Tooltip = "Describe this item"
            Tab = "General"
        },
        
        # Settings Tab
        @{
            Name = "isActive"
            Title = "Active"
            Value = $true
            Editor = "checkbox"
            Tooltip = "Enable or disable this item"
            Tab = "Settings"
        },
        @{
            Name = "priority"
            Title = "Priority"
            Value = 100
            Editor = "number"
            Tooltip = "Sort order priority"
            Tab = "Settings"
        },
        @{
            Name = "publishDate"
            Title = "Publish Date"
            Value = [DateTime]::Now
            Editor = "date time"
            Tooltip = "When to publish"
            Tab = "Settings"
        },
        
        # Advanced Tab
        @{
            Name = "template"
            Title = "Template"
            Root = "/sitecore/templates/"
            Tooltip = "Select a template"
            Tab = "Advanced"
        },
        @{
            Name = "targetUser"
            Title = "Owner"
            Editor = "user"
            Tooltip = "Select the owner"
            Tab = "Advanced"
        }
    )
}

$result = Read-Variable @dialogParams

if ($result -eq "ok") {
    Write-Host "Tabbed Dialog Results:" -ForegroundColor Cyan
    Write-Host ""
    
    Write-Host "General Tab:" -ForegroundColor Green
    Write-Host "  Item Name: $itemName"
    Write-Host "  Title: $itemTitle"
    Write-Host "  Description: $description"
    
    Write-Host ""
    Write-Host "Settings Tab:" -ForegroundColor Green
    Write-Host "  Active: $isActive"
    Write-Host "  Priority: $priority"
    Write-Host "  Publish Date: $publishDate"
    
    Write-Host ""
    Write-Host "Advanced Tab:" -ForegroundColor Green
    Write-Host "  Template: $($template.Name)"
    Write-Host "  Owner: $targetUser"
}

<#
    TAB ORGANIZATION:
    
    Use the Tab parameter to organize controls:
    @{ Name = "field"; Tab = "Tab Name" }
    
    Tips:
    - Tabs appear in the order of their first control
    - Group related controls on the same tab
    - Use clear, concise tab names
    - Limit to 4-5 tabs maximum for usability
    
    Common Tab Patterns:
    - "General" / "Settings" / "Advanced"
    - "Content" / "Metadata" / "Publishing"
    - "Source" / "Target" / "Options"
    - "Basic" / "Security" / "Workflow"
#>
```

**Example:** Conditional visibility

```powershell
<#
    .SYNOPSIS
        Demonstrates conditional visibility of controls in Read-Variable dialogs.
    
    .DESCRIPTION
        Shows how to show/hide controls based on other control values using
        GroupId, ParentGroupId, and HideOnValue parameters.
    
    .NOTES
        Documentation: https://doc.sitecorepowershell.com
#>

# Define options for the controlling dropdown
$modeOptions = [ordered]@{
    "Simple Mode" = "simple"
    "Advanced Mode" = "advanced"
    "Expert Mode" = "expert"
}

$dialogParams = @{
    Title = "Conditional Visibility Example"
    Description = "Controls appear/disappear based on your selection."
    Width = 550
    Height = 450
    OkButtonName = "Continue"
    CancelButtonName = "Cancel"
    ShowHints = $true
    Parameters = @(
        # Checkbox controlling visibility
        @{
            Name = "showAdvanced"
            Title = "Show Advanced Options"
            Value = $true
            Editor = "checkbox"
            Tooltip = "Toggle to show/hide advanced options"
            GroupId = 1  # This control is the "parent" group
        },
        @{
            Name = "advancedOption1"
            Title = "Advanced Option 1"
            Value = ""
            Tooltip = "This field shows when checkbox is checked"
            ParentGroupId = 1  # Linked to group 1
            HideOnValue = "0"  # Hide when checkbox value is 0 (unchecked)
        },
        @{
            Name = "advancedOption2"
            Title = "Advanced Option 2"
            Value = ""
            Tooltip = "This field also shows when checkbox is checked"
            ParentGroupId = 1
            HideOnValue = "0"
        },
        
        # Dropdown controlling visibility
        @{
            Name = "operationMode"
            Title = "Operation Mode"
            Value = "simple"
            Options = $modeOptions
            Editor = "combo"
            Tooltip = "Select mode to see different options"
            GroupId = 2  # This dropdown controls group 2
        },
        @{
            Name = "simpleField"
            Title = "Simple Mode Field"
            Value = ""
            Tooltip = "Visible only in Simple Mode"
            ParentGroupId = 2
            ShowOnValue = "simple"  # Hidden when advanced or expert selected
        },
        @{
            Name = "advancedField"
            Title = "Advanced Mode Field"
            Value = ""
            Tooltip = "Visible only in Advanced Mode"
            ParentGroupId = 2
            ShowOnValue = "advanced"  # Hidden when simple or expert selected
        },
        @{
            Name = "expertField"
            Title = "Expert Mode Field"
            Value = ""
            Tooltip = "Visible only in Expert Mode"
            ParentGroupId = 2
            ShowOnValue = "expert"  # Hidden when simple or advanced selected
        }
    )
}

$result = Read-Variable @dialogParams

if ($result -eq "ok") {
    Write-Host "Conditional Visibility Results:" -ForegroundColor Cyan
    Write-Host ""
    
    Write-Host "Show Advanced: $showAdvanced" -ForegroundColor Green
    if ($showAdvanced) {
        Write-Host "  Advanced Option 1: $advancedOption1"
        Write-Host "  Advanced Option 2: $advancedOption2"
    }
    
    Write-Host ""
    Write-Host "Operation Mode: $operationMode" -ForegroundColor Green
    switch ($operationMode) {
        "simple" { Write-Host "  Simple Field: $simpleField" }
        "advanced" { Write-Host "  Advanced Field: $advancedField" }
        "expert" { Write-Host "  Expert Field: $expertField" }
    }
}

<#
    CONDITIONAL VISIBILITY PARAMETERS:
    
    GroupId:
    - Assigns a numeric ID to a "controlling" control
    - This control's value determines visibility of child controls
    
    ParentGroupId:
    - Links a control to a parent group
    - This control's visibility depends on the parent's value
    
    HideOnValue:
    - Specifies which parent values hide this control
    - For checkboxes: "0" = unchecked, "1" = checked
    - For combos/radios: Use the option values (not display text)
    - Multiple values: Comma-separated list
    
    Common Patterns:
    
    1. Checkbox Toggle:
       Parent: @{ Editor="checkbox"; GroupId=1 }
       Child:  @{ ParentGroupId=1; HideOnValue="0" }  # Show when checked
       Child:  @{ ParentGroupId=1; HideOnValue="1" }  # Show when unchecked
    
    2. Dropdown Selection:
       Parent: @{ Editor="combo"; Options=$opts; GroupId=2 }
       Child:  @{ ParentGroupId=2; HideOnValue="optionA,optionB" }
    
    3. Radio Selection:
       Parent: @{ Editor="radio"; Options=$opts; GroupId=3 }
       Child:  @{ ParentGroupId=3; HideOnValue="1,2" }  # Hide for values 1 and 2
#>
```

**Example:** Column layout

```powershell
<#
    .SYNOPSIS
        Demonstrates column layout in Read-Variable dialogs.
    
    .DESCRIPTION
        Shows how to arrange controls in columns using the Columns parameter.
        The dialog uses a 12-column grid system.
    
    .NOTES
        Documentation: https://doc.sitecorepowershell.com
#>

$dialogParams = @{
    Title = "Column Layout Example"
    Description = "Controls arranged in a responsive grid layout."
    Width = 650
    Height = 500
    OkButtonName = "Save"
    CancelButtonName = "Cancel"
    ShowHints = $true
    Parameters = @(
        # Full width (12 columns - default)
        @{
            Name = "fullWidthField"
            Title = "Full Width Field (12 columns)"
            Value = ""
            Tooltip = "This field spans the full width"
            # Columns = 12 is the default
        },
        
        # Two columns side by side (6 + 6)
        @{
            Name = "firstName"
            Title = "First Name"
            Value = ""
            Tooltip = "Half width"
            Columns = 6
        },
        @{
            Name = "lastName"
            Title = "Last Name"
            Value = ""
            Tooltip = "Half width"
            Columns = 6
        },
        
        # Three columns (4 + 4 + 4)
        @{
            Name = "city"
            Title = "City"
            Value = ""
            Tooltip = "One third width"
            Columns = 4
        },
        @{
            Name = "state"
            Title = "State"
            Value = ""
            Tooltip = "One third width"
            Columns = 4
        },
        @{
            Name = "zip"
            Title = "ZIP Code"
            Value = ""
            Tooltip = "One third width"
            Columns = 4
        },
        
        # Unequal columns (8 + 4)
        @{
            Name = "email"
            Title = "Email Address"
            Value = ""
            Tooltip = "Two thirds width"
            Columns = 8
        },
        @{
            Name = "extension"
            Title = "Ext"
            Value = ""
            Tooltip = "One third width"
            Columns = 4
        },
        
        # Mixed with checkbox (small column)
        @{
            Name = "notes"
            Title = "Additional Notes"
            Value = ""
            Lines = 2
            Tooltip = "Main content area"
            Columns = 9
        },
        @{
            Name = "isUrgent"
            Title = "Urgent"
            Value = $false
            Editor = "checkbox"
            Tooltip = "Mark as urgent"
            Columns = 3
        }
    )
}

$result = Read-Variable @dialogParams

if ($result -eq "ok") {
    Write-Host "Column Layout Results:" -ForegroundColor Cyan
    Write-Host ""
    Write-Host "Full Width: $fullWidthField" -ForegroundColor Green
    Write-Host "Name: $firstName $lastName" -ForegroundColor Green
    Write-Host "Location: $city, $state $zip" -ForegroundColor Green
    Write-Host "Email: $email (ext: $extension)" -ForegroundColor Green
    Write-Host "Notes: $notes" -ForegroundColor Green
    Write-Host "Urgent: $isUrgent" -ForegroundColor Green
}

<#
    COLUMN LAYOUT (12-Column Grid):
    
    The Columns parameter uses a 12-column grid system:
    
    Full Width:    Columns = 12 (default)
    Half Width:    Columns = 6
    Third Width:   Columns = 4
    Quarter Width: Columns = 3
    Two-Thirds:    Columns = 8
    
    Common Patterns:
    
    Two Equal Columns:
    @{ Columns = 6 }, @{ Columns = 6 }
    
    Three Equal Columns:
    @{ Columns = 4 }, @{ Columns = 4 }, @{ Columns = 4 }
    
    Main + Sidebar:
    @{ Columns = 8 }, @{ Columns = 4 }
    
    Four Equal Columns:
    @{ Columns = 3 }, @{ Columns = 3 }, @{ Columns = 3 }, @{ Columns = 3 }
    
    Tips:
    - Column values should add up to 12 for a complete row
    - Controls wrap to next row automatically
    - Combine with tabs for complex layouts
    - Consider dialog width when using many columns
#>
```

**Example:** Mandatory fields

```powershell
<#
    .SYNOPSIS
        Demonstrates mandatory fields and validation in Read-Variable dialogs.
    
    .DESCRIPTION
        Shows how to mark fields as required using the Mandatory parameter.
        Required fields must have a value before the dialog can be submitted.
    
    .NOTES
        Documentation: https://doc.sitecorepowershell.com
#>

# Initialize item variable - required for item picker
$requiredItem = Get-Item -Path "master:\content\home"

$dialogParams = @{
    Title = "Required Fields Example"
    Description = "Fields marked with * are required."
    Width = 500
    Height = 400
    OkButtonName = "Submit"
    CancelButtonName = "Cancel"
    ShowHints = $true
    Parameters = @(
        @{
            Name = "requiredField"
            Title = "Required Text Field"
            Value = ""
            Tooltip = "This field must be filled in"
            Placeholder = "This field is required..."
            Mandatory = $true
        },
        @{
            Name = "optionalField"
            Title = "Optional Text Field"
            Value = ""
            Tooltip = "This field is optional"
            Placeholder = "This field is optional..."
            Mandatory = $false
        },
        @{
            Name = "requiredNumber"
            Title = "Required Number"
            Value = $null
            Editor = "number"
            Tooltip = "A number must be entered"
            Mandatory = $true
        },
        @{
            Name = "requiredItem"
            Title = "Required Item Selection"
            Root = "/sitecore/content/"
            Tooltip = "An item must be selected"
            Mandatory = $true
        },
        @{
            Name = "optionalMultiline"
            Title = "Optional Notes"
            Value = ""
            Lines = 3
            Tooltip = "Additional notes (optional)"
            Mandatory = $false
        }
    )
}

$result = Read-Variable @dialogParams

if ($result -eq "ok") {
    Write-Host "Form submitted successfully!" -ForegroundColor Cyan
    Write-Host ""
    Write-Host "Required Field: $requiredField" -ForegroundColor Green
    Write-Host "Optional Field: $optionalField" -ForegroundColor Green
    Write-Host "Required Number: $requiredNumber" -ForegroundColor Green
    
    if ($requiredItem) {
        Write-Host "Required Item: $($requiredItem.Name)" -ForegroundColor Green
    }
    
    Write-Host "Optional Notes: $optionalMultiline" -ForegroundColor Green
} else {
    Write-Host "Dialog was cancelled" -ForegroundColor Yellow
}

<#
    MANDATORY FIELDS:
    
    The Mandatory parameter enforces that a field has a value:
    @{ Name = "field"; Mandatory = $true }
    
    Behavior:
    - Required fields show an asterisk (*) in the label
    - Dialog cannot be submitted until all required fields have values
    - An error message appears if user tries to submit with empty required fields
    - Default is Mandatory = $false
    
    Supported Field Types:
    - Text fields (single and multi-line)
    - Number fields
    - Item pickers
    - User/Role selectors
    - Dropdowns and lists
    - Date/time fields
    
    Notes:
    - Checkboxes don't support Mandatory (they always have a value)
    - For item pickers, the user must select an item
    - Empty strings "" are considered invalid for mandatory text fields
    - Consider using Placeholder text to guide users on required fields
#>
```

**Example:** Dialog customization

```powershell
<#
    .SYNOPSIS
        Demonstrates dialog customization options in Read-Variable.
    
    .DESCRIPTION
        Shows how to customize dialog appearance including title, description,
        icon, dimensions, button labels, and hint display.
    
    .NOTES
        Documentation: https://doc.sitecorepowershell.com
#>

$dialogParams = @{
    # Dialog Title - appears in the title bar
    Title = "Custom Dialog Example"
    
    # Description - appears below the title
    Description = "This dialog demonstrates all available customization options for Read-Variable dialogs."
    
    # Icon - Sitecore icon path (various icon sets available)
    Icon = "Office/32x32/coffee.png"
    
    # Dimensions
    Width = 550
    Height = 400
    
    # Button Labels
    OkButtonName = "Process Items"
    CancelButtonName = "Abort"
    
    # Show tooltips for controls
    ShowHints = $true
    
    # Controls
    Parameters = @(
        @{
            Name = "inputText"
            Title = "Sample Input"
            Value = ""
            Tooltip = "This tooltip appears when ShowHints is enabled"
            Placeholder = "Enter something..."
        },
        @{
            Name = "confirm"
            Title = "I understand the implications"
            Value = $false
            Editor = "checkbox"
            Tooltip = "Check to confirm"
        }
    )
}

$result = Read-Variable @dialogParams

if ($result -eq "ok") {
    Write-Host "Dialog completed with custom settings!" -ForegroundColor Green
    Write-Host "Input: $inputText"
    Write-Host "Confirmed: $confirm"
} else {
    Write-Host "User clicked the custom cancel button" -ForegroundColor Yellow
}

<#
    DIALOG CUSTOMIZATION OPTIONS:
    
    Title (string):
    - Text shown in the dialog title bar
    - Keep concise but descriptive
    
    Description (string):
    - Explanatory text below the title
    - Use for instructions or context
    - Can span multiple lines
    
    Icon (string):
    - Sitecore icon path
    - Format: "IconSet/Size/iconname.png"
    - Common sets: Office, Business, Software, People, Officewhite
    - Sizes: 16x16, 24x24, 32x32, 48x48
    - Examples:
      - "Office/32x32/document.png"
      - "Business/32x32/users.png"
      - "Software/32x32/gear.png"
      - "Officewhite/32x32/knife_fork_spoon.png"
    
    Width / Height (integer):
    - Dialog dimensions in pixels
    - Minimum recommended: 400x300
    - Default: 500x500 approximately
    
    OkButtonName (string):
    - Custom label for the OK/Submit button
    - Examples: "Continue", "Process", "Save", "Apply"
    
    CancelButtonName (string):
    - Custom label for the Cancel button
    - Examples: "Abort", "Skip", "Close", "Back"
    
    ShowHints (boolean):
    - When true, tooltips appear for controls with Tooltip set
    - Recommended: $true for complex dialogs
    
    Return Values:
    - "ok" - User clicked the OK button
    - "cancel" - User clicked Cancel or closed the dialog
#>
```

**Example:** Simple dialog

```powershell
<#
    .SYNOPSIS
        Demonstrates a simple dialog without tabs in Read-Variable.
    
    .DESCRIPTION
        Shows how to create a straightforward dialog for common operations
        without using tabs or complex layouts.
    
    .NOTES
        Documentation: https://doc.sitecorepowershell.com
#>

# Initialize item variable - required for item picker
$targetItem = Get-Item -Path "master:\content\home"

$dialogParams = @{
    Title = "Simple Dialog Example"
    Description = "A straightforward dialog for quick data collection."
    Width = 500
    Height = 450
    OkButtonName = "Finish"
    CancelButtonName = "Cancel"
    Parameters = @(
        @{ 
            Name = "inputText"
            Value = "Some Text"
            Title = "Single Line Text"
            Tooltip = "Enter a single line of text"
            Placeholder = "Enter text here..."
        },
        @{ 
            Name = "multiLineText"
            Value = "Multiple lines\nof text"
            Title = "Multi Line Text"
            Lines = 3
            Tooltip = "Enter multiple lines"
            Placeholder = "Enter detailed text..."
        },
        @{ 
            Name = "startDate"
            Value = [System.DateTime]::Now.AddDays(-5)
            Title = "Start Date"
            Tooltip = "Select a start date"
            Editor = "date time"
        },
        @{ 
            Name = "selectedUser"
            Value = ""
            Title = "Select User"
            Tooltip = "Choose a user for this operation"
            Editor = "user multiple"
        },
        @{ 
            Name = "targetItem"
            Title = "Target Item"
            Root = "/sitecore/content/"
            Tooltip = "Select the target location"
        }
    )
}

$result = Read-Variable @dialogParams

if ($result -ne "ok") {
    Write-Host "Operation cancelled by user" -ForegroundColor Yellow
    Exit
}

# Process the collected data
Write-Host "Dialog Results:" -ForegroundColor Cyan
Write-Host ""
Write-Host "Text: $inputText" -ForegroundColor Green
Write-Host "Multi-line: $multiLineText" -ForegroundColor Green
Write-Host "Start Date: $startDate" -ForegroundColor Green
Write-Host "User: $selectedUser" -ForegroundColor Green

if ($targetItem) {
    Write-Host "Target Item: $($targetItem.ItemPath)" -ForegroundColor Green
}

<#
    SIMPLE DIALOG PATTERN:
    
    For many scripts, a simple dialog without tabs is sufficient.
    This pattern is ideal when you have:
    - 5-8 related fields
    - A single, focused task
    - No need for complex organization
    
    Best Practices:
    1. Order fields logically (most important first)
    2. Use clear, descriptive titles
    3. Provide helpful tooltips
    4. Use placeholder text for text inputs
    5. Set sensible default values
    6. Keep the dialog focused on one task
    
    When to Use Tabs Instead:
    - More than 8-10 fields
    - Fields can be grouped into logical categories
    - Complex operations with multiple phases
    - Advanced options that most users won't need
#>
```

**Example:** Comprehensive example

```powershell
<#
    .SYNOPSIS
        Comprehensive example combining multiple dialog features.
    
    .DESCRIPTION
        Demonstrates how to combine tabs, columns, conditional visibility,
        and various control types in a real-world scenario.
    
    .NOTES
        Documentation: https://doc.sitecorepowershell.com
#>

# Initialize item variable - required for item picker
$sourceItem = Get-Item -Path "master:\content\home"

# Pre-define some default values
$publishModes = [ordered]@{
    "Smart Publish" = "smart"
    "Republish" = "republish"
    "Incremental" = "incremental"
}

$targetDatabases = [ordered]@{
    "Web" = 1
    "Preview" = 2
}

$languageOptions = [ordered]@{
    "English" = 1
    "German" = 2
    "French" = 4
    "Spanish" = 8
}

$selectedLanguages = @(1)  # English pre-selected

# Build the dialog parameters
$dialogParams = @{
    Title = "Content Publishing Wizard"
    Description = "Configure and execute content publishing operations."
    Icon = "Office/32x32/document_up.png"
    Width = 650
    Height = 600
    OkButtonName = "Publish"
    CancelButtonName = "Cancel"
    ShowHints = $true
    Parameters = @(
        # === SOURCE TAB ===
        @{
            Name = "sourceItem"
            Title = "Source Item"
            Root = "/sitecore/content/"
            Tooltip = "Select the root item to publish"
            Tab = "Source"
            Mandatory = $true
        },
        @{
            Name = "includeSubitems"
            Title = "Include Subitems"
            Value = $true
            Editor = "checkbox"
            Tooltip = "Publish all descendant items"
            Tab = "Source"
            Columns = 6
        },
        @{
            Name = "includeRelated"
            Title = "Include Related Items"
            Value = $false
            Editor = "checkbox"
            Tooltip = "Also publish referenced items"
            Tab = "Source"
            Columns = 6
        },
        
        # === TARGETS TAB ===
        @{
            Name = "targetDbs"
            Title = "Target Databases"
            Options = $targetDatabases
            Editor = "checklist"
            Tooltip = "Select destination databases"
            Tab = "Targets"
        },
        @{
            Name = "selectedLanguages"
            Title = "Languages"
            Options = $languageOptions
            Editor = "checklist"
            Tooltip = "Select languages to publish"
            Tab = "Targets"
        },
        
        # === OPTIONS TAB ===
        @{
            Name = "publishMode"
            Title = "Publish Mode"
            Value = "smart"
            Options = $publishModes
            Editor = "radio"
            Tooltip = "Select publishing mode"
            Tab = "Options"
        },
        @{
            Name = "schedulePublish"
            Title = "Schedule for Later"
            Value = $false
            Editor = "checkbox"
            Tooltip = "Check to schedule publication"
            Tab = "Options"
            GroupId = 1
        },
        @{
            Name = "scheduledTime"
            Title = "Scheduled Time"
            Value = [DateTime]::Now.AddHours(1)
            Editor = "date time"
            Tooltip = "When to publish"
            Tab = "Options"
            ParentGroupId = 1
            HideOnValue = "0"
        },
        
        # === NOTIFICATIONS TAB ===
        @{
            Name = "sendNotification"
            Title = "Send Notification"
            Value = $false
            Editor = "checkbox"
            Tooltip = "Notify users when complete"
            Tab = "Notifications"
            GroupId = 2
        },
        @{
            Name = "notifyUsers"
            Title = "Notify Users"
            Editor = "user multiple"
            Tooltip = "Select users to notify"
            Tab = "Notifications"
            ParentGroupId = 2
            HideOnValue = "0"
        },
        @{
            Name = "notifyRoles"
            Title = "Notify Roles"
            Editor = "role multiple"
            Domain = "sitecore"
            Tooltip = "Select roles to notify"
            Tab = "Notifications"
            ParentGroupId = 2
            HideOnValue = "0"
        },
        @{
            Name = "notificationMessage"
            Title = "Custom Message"
            Value = ""
            Lines = 3
            Tooltip = "Add a custom message to the notification"
            Tab = "Notifications"
            ParentGroupId = 2
            HideOnValue = "0"
            Placeholder = "Optional message to include..."
        }
    )
}

$result = Read-Variable @dialogParams

if ($result -ne "ok") {
    Write-Host "Publishing cancelled by user" -ForegroundColor Yellow
    Exit
}

# Display collected settings
Write-Host ("=" * 50) -ForegroundColor Cyan
Write-Host "PUBLISHING CONFIGURATION" -ForegroundColor Cyan
Write-Host ("=" * 50) -ForegroundColor Cyan
Write-Host ""

Write-Host "SOURCE:" -ForegroundColor Green
Write-Host "  Item: $($sourceItem.ItemPath)"
Write-Host "  Include Subitems: $includeSubitems"
Write-Host "  Include Related: $includeRelated"
Write-Host ""

Write-Host "TARGETS:" -ForegroundColor Green
Write-Host "  Databases: $($targetDbs -join ', ')"
Write-Host "  Languages: $($selectedLanguages -join ', ')"
Write-Host ""

Write-Host "OPTIONS:" -ForegroundColor Green
Write-Host "  Mode: $publishMode"
if ($schedulePublish) {
    Write-Host "  Scheduled: $scheduledTime"
} else {
    Write-Host "  Scheduled: Immediate"
}
Write-Host ""

if ($sendNotification) {
    Write-Host "NOTIFICATIONS:" -ForegroundColor Green
    Write-Host "  Users: $($notifyUsers -join ', ')"
    Write-Host "  Roles: $($notifyRoles -join ', ')"
    if ($notificationMessage) {
        Write-Host "  Message: $notificationMessage"
    }
}

Write-Host ""
Write-Host "Ready to execute publishing operation!" -ForegroundColor Yellow
```