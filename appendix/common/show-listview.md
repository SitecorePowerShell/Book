# Show-ListView

Sends output to an interactive table in a separate window.

## Syntax

Show-ListView \[-PageSize &lt;Int32&gt;\] \[-Modal\] \[-ActionData &lt;Object&gt;\] \[-ViewName &lt;String&gt;\] \[-ActionsInSession\] \[-Show &lt;None \| SharedExport \| Filter \| PagingAlways \| SharedActions \| StatusBar \| All&gt;\] \[-InfoTitle &lt;String&gt;\] \[-InfoDescription &lt;String&gt;\] \[-MissingDataMessage &lt;String&gt;\] \[-Icon &lt;String&gt;\] -Data &lt;Object&gt; \[-Property &lt;Object\[\]&gt;\] \[-Title &lt;String&gt;\] \[-Width &lt;Int32&gt;\] \[-Height &lt;Int32&gt;\]

## Detailed Description

The Show-ListView command sends the output from a command to a grid view window where the output is displayed in an interactive table. Because this command requires a user interface, it does not work in a non-interactive scenarios like within web service calls. You can use the following features of the table to examine your data: -- Sort. To sort the data, click a column header. Click again to toggle from ascending to descending order. -- Quick Filter. Use the "Filter" box at the top of the window to search the text in the table. You can search for text in a particular column, search for literals, and search for multiple words. -- Execute actions on selected items. To execute action on the data from Show-ListView, Ctrl+click the items you want to be included in the action and press the desired action in the "Actions" chunk in the ribbon. -- Export contents of the view in XML, CSV, Json, HTML or Excel file and download that onto the user's computer. The downloaded results will take into account current filter and order of the items.

© 2010-2019 Adam Najmanowicz, Michael West. All rights reserved. Sitecore PowerShell Extensions

## Parameters

### -PageSize  &lt;Int32&gt;

Number of results shown per page.

| Aliases |  |
| :--- | :--- |
| Required? | false |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | false |
| Accept Wildcard Characters? | false |

### -Modal  &lt;SwitchParameter&gt;

If this parameter is provided Results will show in a new window \(in Sitecore 6.x up till Sitecore 7.1\) or in a modal overlay \(Sitecore 7.2+\)

| Aliases |  |
| :--- | :--- |
| Required? | false |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | false |
| Accept Wildcard Characters? | false |

### -ActionData  &lt;Object&gt;

Additional data what will be passed to the view. All actions that are executed from that view window will have that data accessible to them as $actionData variable.

| Aliases |  |
| :--- | :--- |
| Required? | false |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | false |
| Accept Wildcard Characters? | false |

### -ViewName  &lt;String&gt;

View signature name - this can be used by action commands to determine whether to show an action or not using the Show/Enable rules.

| Aliases |  |
| :--- | :--- |
| Required? | false |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | false |
| Accept Wildcard Characters? | false |

### -ActionsInSession  &lt;SwitchParameter&gt;

If this parameter is specified actions will be executed in the same session as the one in which the command is executed.

| Aliases |  |
| :--- | :--- |
| Required? | false |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | false |
| Accept Wildcard Characters? | false |

### -Show  &lt;ShowListViewFeatures&gt;

Shows UI elements selectively in the results dialog -- All - shows all UI elements automatically - default value -- SharedExport - shows export filters that are not specific to this very `-ViewName report (left-most ribbon panel) -- Filter - shows filter panel -- PagingAlways - shows paging when list is shorter than the page specified -- SharedActions - shows actions that are not specific to this very`-ViewName report \(right-most ribbon panel\) -- StatusBar - shows status bar.

| Aliases |  |
| :--- | :--- |
| Required? | false |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | false |
| Accept Wildcard Characters? | false |

### -InfoTitle  &lt;String&gt;

Title on the panel that appears below the ribbon in the results window.

| Aliases |  |
| :--- | :--- |
| Required? | false |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | false |
| Accept Wildcard Characters? | false |

### -InfoDescription  &lt;String&gt;

Description that appears on the panel below the ribbon in the results window.

| Aliases |  |
| :--- | :--- |
| Required? | false |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | false |
| Accept Wildcard Characters? | false |

### -MissingDataMessage  &lt;String&gt;

If no Items were provided for -Data parameter the message provided in this parameter will be shown in the middle of the List View dialog to notify users of the lack of items to display.

| Aliases |  |
| :--- | :--- |
| Required? | false |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | false |
| Accept Wildcard Characters? | false |

### -Icon  &lt;String&gt;

Icon of the result window. \(Shows in the top/left corner and on the Sitecore taskbar\).

| Aliases |  |
| :--- | :--- |
| Required? | false |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | false |
| Accept Wildcard Characters? | false |

### -Data  &lt;Object&gt;

Data to be displayed in the view.

| Aliases |  |
| :--- | :--- |
| Required? | true |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | true \(ByValue\) |
| Accept Wildcard Characters? | false |

### -Property  &lt;Object\[\]&gt;

Specifies the object properties that appear in the display and the order in which they appear. Type one or more property names \(separated by commas\), or use a hash table to display a calculated property.

The value of the Property parameter can be a new calculated property. To create a calculated, property, use a hash table. Valid keys are: -- Name \(or Label\) &lt;string&gt; -- Expression &lt;string&gt; or &lt;script block&gt;

| Aliases |  |
| :--- | :--- |
| Required? | false |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | false |
| Accept Wildcard Characters? | false |

### -Title  &lt;String&gt;

Title of the results window.

| Aliases |  |
| :--- | :--- |
| Required? | false |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | false |
| Accept Wildcard Characters? | false |

### -Width  &lt;Int32&gt;

Width of the results window. Only applicable when also using the `-Modal` switch.

| Aliases |  |
| :--- | :--- |
| Required? | false |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | false |
| Accept Wildcard Characters? | false |

### -Height  &lt;Int32&gt;

Height of the results window. Only applicable when also using the `-Modal` switch.

| Aliases |  |
| :--- | :--- |
| Required? | false |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | false |
| Accept Wildcard Characters? | false |

## Inputs

The input type is the type of the objects that you can pipe to the cmdlet.

* System.Management.Automation.PSObject 

## Outputs

The output type is the type of the objects that the cmdlet emits.

* System.String 

## Notes

Help Author: Adam Najmanowicz, Michael West

## Examples

### EXAMPLE 1

This command formats information about Sitecore items in a table. The Get-ChildItem command gets objects representing the items. The pipeline operator \(\|\) passes the object to the Show-ListView command. Show-ListView displays the objects in a table.

```powershell
PS master:\> Get-Item -path master:\* | Show-ListView -Property Name, DisplayName, ProviderPath, TemplateName, Language
```

### EXAMPLE 2

This command formats information about Sitecore items in a table. The Get-ItemReferrer command gets all references of the "Sample Item" template. The pipeline operator \(\|\) passes the object to the Show-ListView command. Show-ListView displays the objects in a table. The Properties are not displaying straight properties but use the Name/Expression scheme to provide a nicely named values that like in the case of languages which are aggregarde form the "Languages" property.

```powershell
PS master:\> Get-ItemReferrer -path 'master:\templates\Sample\Sample Item' | 
                 Show-ListView -Property `
                     @{Label="Name"; Expression={$_.DisplayName} }, 
                     @{Label="Path"; Expression={$_.Paths.Path} }, 
                     @{Label="Languages"; Expression={$_.Languages | % { $_.Name + ", "} } }
```

### EXAMPLE 3

The following demonstrates the use of a custom object used in the report. Notice that a custom Icon can be set.

```powershell
$item = Get-Item -Path "master:\content\home\sample item 1"
 
$customItem = [pscustomobject]@{
    "ID"=$Item.ID
    "Icon"=$Item.__Icon
    "DisplayName"=$Item.DisplayName
    "ItemPath"=$Item.ItemPath
    "Version"=$Item.Version
    "Language"=$Item.Language
    "__Updated"=$Item.__Updated
    "__Updated by"=$Item."__Updated by"
}

$customItem | Show-ListView
```

## Related Topics

* [Update-ListView](update-listview.md)
* Out-GridView
* Format-Table
* [Read-Variable](read-variable.md)
* [Show-Alert](show-alert.md)
* [Show-Application](show-application.md)
* [Show-Confirm](show-confirm.md)
* [Show-FieldEditor](show-fieldeditor.md)
* [Show-Input](show-input.md)
* [Show-ModalDialog](show-modaldialog.md)
* [Show-Result](show-result.md)
* [Show-YesNoCancel](show-yesnocancel.md)
* [https://blog.najmanowicz.com/2014/10/25/creating-beautiful-sitecore-reports-easily-with-powershell-extensions/](https://blog.najmanowicz.com/2014/10/25/creating-beautiful-sitecore-reports-easily-with-powershell-extensions/) 
* [https://michaellwest.blogspot.com/2014/04/reports-with-sitecore-powershell.html](https://michaellwest.blogspot.com/2014/04/reports-with-sitecore-powershell.html) 
* [https://sitecorejunkie.com/2014/05/28/create-a-custom-report-in-sitecore-powershell-extensions/](https://sitecorejunkie.com/2014/05/28/create-a-custom-report-in-sitecore-powershell-extensions/) 
* [https://github.com/SitecorePowerShell/Console/](https://github.com/SitecorePowerShell/Console/) 

