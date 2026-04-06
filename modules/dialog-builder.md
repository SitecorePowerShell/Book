# DialogBuilder

{% hint style="info" %}
Introduced in SPE 9.0.
{% endhint %}

The DialogBuilder extension library provides a fluent, pipeline-based API for constructing [`Read-Variable`](../appendix/common/read-variable.md) dialogs. Instead of building verbose hashtable arrays and manually initializing variables, you chain intuitive commands that handle type inference, variable initialization, and editor selection automatically.

## Getting Started

Load the DialogBuilder library with `Import-Function`:

```powershell
Import-Function -Name DialogBuilder
```

A typical workflow is: create a builder, add fields, then invoke the dialog.

```powershell
Import-Function -Name DialogBuilder

$dialog = New-DialogBuilder -Title "Content Editor" -ShowHints
$dialog | Add-TextField -Name "userName" -Title "User Name" -Mandatory
$dialog | Add-Checkbox -Name "publish" -Title "Publish after save"

$result = $dialog | Invoke-Dialog
if ($result.Result -eq "ok") {
    Write-Host "User: $userName, Publish: $publish"
}
```

{% hint style="info" %}
Variables defined with `-Name` in field functions (e.g., `$userName`, `$publish`) are automatically created in your script scope when `Invoke-Dialog` is called, and their values are updated based on user input.
{% endhint %}

## Core Commands

| Command | Description |
| :--- | :--- |
| `New-DialogBuilder` | Creates a new builder instance for constructing `Read-Variable` dialogs. |
| `Invoke-Dialog` | Executes the dialog and returns a structured result object. |
| `Test-DialogBuilder` | Validates the dialog configuration without displaying it. |
| `Copy-DialogBuilder` | Creates a deep copy of a builder instance. |
| `ConvertTo-DialogBuilderJson` | Exports dialog configuration to JSON format. |
| `Import-DialogBuilderConfig` | Imports dialog configuration from JSON. |

### New-DialogBuilder

Creates a new dialog builder instance.

| Parameter | Description |
| :--- | :--- |
| `-Title` | The dialog title shown in the title bar. |
| `-Description` | Descriptive text shown below the title. |
| `-Width` | Dialog width in pixels (default: 500). |
| `-Height` | Dialog height in pixels (default: 400). |
| `-OkButtonName` | Custom text for the OK button (default: "OK"). |
| `-CancelButtonName` | Custom text for the Cancel button (default: "Cancel"). |
| `-ShowHints` | Display tooltips for fields. |
| `-Icon` | Sitecore icon path (e.g., `"Office/32x32/document.png"`). |

### Invoke-Dialog

Executes the dialog and returns a structured `PSObject` with the following properties:

| Property | Description |
| :--- | :--- |
| `Result` | `"ok"` if user clicked OK, `"cancel"` if cancelled. |
| `Title` | The dialog title. |
| `FieldCount` | Number of fields in the dialog. |

```powershell
$result = $dialog | Invoke-Dialog
if ($result.Result -eq "ok") {
    # Field variables are now available in scope
}
```

**Validation:** Pass a `-Validator` scriptblock to validate fields before accepting the dialog. Access field values via `$variables.<fieldName>.Value` and set errors via `$variables.<fieldName>.Error`.

```powershell
$validator = {
    if ($variables.password.Value.Length -lt 8) {
        $variables.password.Error = "Password must be at least 8 characters"
    }
}
$result = $dialog | Invoke-Dialog -Validator $validator
```

{% hint style="warning" %}
Validator scriptblocks are not supported under Constrained Language Mode (CLM).
{% endhint %}

### Test-DialogBuilder

Validates the dialog configuration without displaying it. Checks for duplicate field names, orphaned ParentGroupIds, and performance concerns.

| Parameter | Description |
| :--- | :--- |
| `-WarningsOnly` | Only show warnings, suppress info messages. |
| `-ShowDetails` | Show detailed debug information about the dialog structure. |
| `-ShowValues` | Include current field values in the output. |
| `-ShowFullParameters` | Display all parameter details for each field. |

```powershell
$isValid = $dialog | Test-DialogBuilder
if ($isValid) {
    $dialog | Invoke-Dialog
}

# Detailed debug output
$dialog | Test-DialogBuilder -ShowDetails -ShowValues
```

### ConvertTo-DialogBuilderJson

Exports dialog configuration to a versioned JSON format with metadata (version, timestamp, user, SPE version).

```powershell
$json = $dialog | ConvertTo-DialogBuilderJson
$dialog | ConvertTo-DialogBuilderJson -FilePath "$SitecoreDataFolder\dialog.json"
```

### Import-DialogBuilderConfig

Reconstructs a dialog builder from exported JSON.

```powershell
$dialog = Import-DialogBuilderConfig -FilePath "dialog.json"
$dialog = Import-DialogBuilderConfig -Json $jsonString
```

## Field Commands

Field commands add controls to the dialog. They accept pipeline input from the builder and mutate it in-place (void return).

### Add-DialogField

The core field command with full control over all parameters.

| Parameter | Description |
| :--- | :--- |
| `-Name` | Variable name (without `$`). Becomes the PowerShell variable name. |
| `-Title` | Field label shown to the user. |
| `-Value` | Initial/default value. Item types are auto-initialized. |
| `-Tooltip` | Help text shown when `ShowHints` is enabled. |
| `-Placeholder` | Placeholder text for empty text fields. |
| `-Editor` | Explicit editor type. Auto-detected if not specified. |
| `-Mandatory` | Makes this field required. |
| `-Tab` | Tab name for organizing fields. |
| `-Columns` | Column width in 12-column grid (default: 12). |
| `-Lines` | Number of lines for multi-line text fields. |
| `-Root` | Root path for item picker controls. |
| `-Source` | Data source configuration for complex controls. |
| `-Domain` | Domain filter for user/role pickers. |
| `-Options` | Options hashtable for combo/radio/checklist controls. |
| `-GroupId` | ID for conditional visibility grouping. |
| `-ParentGroupId` | Parent group ID for conditional visibility. |
| `-HideOnValue` / `-ShowOnValue` | Values that hide/show this field based on parent. |

### Remove-DialogField

Removes a field from the dialog by name. Void return.

```powershell
$dialog | Remove-DialogField -Name "optionalField"
```

### Convenience Field Commands

These wrap `Add-DialogField` with sensible defaults for specific control types.

#### Text Input

| Command | Description |
| :--- | :--- |
| `Add-TextField` | Single-line text input. Supports `-IsPassword`, `-IsEmail`, `-IsNumber` parameter sets. |
| `Add-MultiLineTextField` | Multi-line text area. Use `-Lines` to set height. |
| `Add-LinkField` | URL input field. |

```powershell
$dialog | Add-TextField -Name "firstName" -Title "First Name" -Mandatory -Columns 6
$dialog | Add-TextField -Name "password" -Title "Password" -IsPassword
$dialog | Add-TextField -Name "email" -Title "Email" -IsEmail -Placeholder "user@example.com"
$dialog | Add-TextField -Name "age" -Title "Age" -IsNumber -Value 25
$dialog | Add-MultiLineTextField -Name "notes" -Title "Notes" -Lines 5
$dialog | Add-LinkField -Name "url" -Title "Website"
```

#### Boolean

| Command | Description |
| :--- | :--- |
| `Add-Checkbox` | Standard checkbox (true/false). |
| `Add-TristateCheckbox` | Three-state checkbox (true/false/indeterminate). |

#### Selection

| Command | Description |
| :--- | :--- |
| `Add-RadioButtons` | Radio button group from an `-Options` hashtable. |
| `Add-Dropdown` | Dropdown (combo) from an `-Options` hashtable. |
| `Add-Checklist` | Multi-select checklist from an `-Options` hashtable. |

```powershell
$dialog | Add-RadioButtons -Name "color" -Title "Color" -Options @{
    "red" = "Red"
    "blue" = "Blue"
    "green" = "Green"
}
```

#### Date/Time

| Command | Description |
| :--- | :--- |
| `Add-DateTimePicker` | Date and time picker. Use `-DateOnly` for date-only selection. |

```powershell
$dialog | Add-DateTimePicker -Name "startDate" -Title "Start Date" -DateOnly
$dialog | Add-DateTimePicker -Name "deadline" -Title "Deadline" -Value ([DateTime]::Now.AddDays(30))
```

#### Item Pickers

These controls return Sitecore Item objects:

| Command | Description |
| :--- | :--- |
| `Add-ItemPicker` | Standard item picker with browse dialog. |
| `Add-Droplink` | Dropdown that returns an Item object. |
| `Add-Droptree` | Tree picker that returns an Item object. |
| `Add-GroupedDroplink` | Grouped dropdown that returns an Item object. |

These controls return string values:

| Command | Description |
| :--- | :--- |
| `Add-Droplist` | Dropdown that returns a string value. |
| `Add-GroupedDroplist` | Grouped dropdown that returns a string value. |

Multi-select item pickers:

| Command | Description |
| :--- | :--- |
| `Add-TreeList` | Multi-select tree list. Use `-WithSearch` for searchable. |
| `Add-MultiList` | Multi-select bucket list. Use `-WithSearch` for searchable. |

```powershell
$dialog | Add-ItemPicker -Name "rootItem" -Title "Root Item" -Root "/sitecore/content"
$dialog | Add-Droplink -Name "template" -Title "Template" -Source "DataSource=/sitecore/templates"
$dialog | Add-TreeList -Name "items" -Title "Selected Items" -Root "/sitecore/content" -WithSearch
```

#### User/Role Pickers

| Command | Description |
| :--- | :--- |
| `Add-UserPicker` | User selection control. |
| `Add-RolePicker` | Role selection control. |
| `Add-UserRolePicker` | Combined user and role selection. |

#### Rules

| Command | Description |
| :--- | :--- |
| `Add-RuleField` | Sitecore Rules Engine field. |
| `Add-RuleActionField` | Rules action-only field. |

#### Display

| Command | Description |
| :--- | :--- |
| `Add-InfoText` | Read-only informational text. |
| `Add-Marquee` | Scrolling marquee text. |

## Layout and Organization

### Tabs

Organize fields into tabs using the `-Tab` parameter:

```powershell
$dialog | Add-TextField -Name "firstName" -Title "First Name" -Tab "Personal"
$dialog | Add-TextField -Name "lastName" -Title "Last Name" -Tab "Personal"
$dialog | Add-TextField -Name "company" -Title "Company" -Tab "Work"
```

### Columns

Use the `-Columns` parameter with a 12-column grid for side-by-side layout:

```powershell
$dialog | Add-TextField -Name "firstName" -Title "First Name" -Columns 6
$dialog | Add-TextField -Name "lastName" -Title "Last Name" -Columns 6
```

### Conditional Visibility

Show or hide fields based on another field's value using `-GroupId` and `-ParentGroupId`:

```powershell
$dialog | Add-Checkbox -Name "showAdvanced" -Title "Show Advanced" -GroupId 1
$dialog | Add-TextField -Name "advancedSetting" -Title "Setting" -ParentGroupId 1 -HideOnValue "0"
```

## Examples

### Complete Dialog with Tabs

```powershell
Import-Function -Name DialogBuilder

$dialog = New-DialogBuilder -Title "Content Editor" -Width 700 -Height 600 -ShowHints -Icon "Office/32x32/document.png"

# Input tab
$dialog | Add-TextField -Name "title" -Title "Title" -Mandatory -Tab "Content"
$dialog | Add-MultiLineTextField -Name "body" -Title "Body" -Lines 5 -Tab "Content"
$dialog | Add-DateTimePicker -Name "publishDate" -Title "Publish Date" -DateOnly -Tab "Content"

# Settings tab
$dialog | Add-ItemPicker -Name "location" -Title "Location" -Root "/sitecore/content" -Tab "Settings"
$dialog | Add-Checkbox -Name "publish" -Title "Publish after save" -Tab "Settings"
$dialog | Add-Dropdown -Name "language" -Title "Language" -Tab "Settings" -Options @{
    "en" = "English"
    "fr-FR" = "French"
    "de-DE" = "German"
}

$result = $dialog | Invoke-Dialog
if ($result.Result -eq "ok") {
    Write-Host "Title: $title"
    Write-Host "Publish: $publish, Language: $language"
}
```

### Dynamic Dialog Construction

```powershell
Import-Function -Name DialogBuilder

$dialog = New-DialogBuilder -Title "Dynamic Form"
$dialog | Add-TextField -Name "name" -Title "Name" -Mandatory
$dialog | Add-TextField -Name "optional" -Title "Optional Field"

# Conditionally remove a field
if (-not $showOptional) {
    $dialog | Remove-DialogField -Name "optional"
}

# Validate before showing
if ($dialog | Test-DialogBuilder) {
    $result = $dialog | Invoke-Dialog
}
```

### Export and Import Configuration

```powershell
Import-Function -Name DialogBuilder

# Build and export
$dialog = New-DialogBuilder -Title "Reusable Dialog"
$dialog | Add-TextField -Name "name" -Title "Name"
$dialog | ConvertTo-DialogBuilderJson -FilePath "$SitecoreDataFolder\my-dialog.json"

# Import and reuse
$imported = Import-DialogBuilderConfig -FilePath "$SitecoreDataFolder\my-dialog.json"
$result = $imported | Invoke-Dialog
```

## Related Topics

- [Read-Variable](../appendix/common/read-variable.md) — The underlying command that DialogBuilder wraps
- [Interactive Dialogs](../interfaces/interactive-dialogs.md) — Overview of SPE dialog capabilities
