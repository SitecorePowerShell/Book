# Glossary

## CLI

Command-line interface

## IIS

Internet Information Services

## ISE

Integrated Scripting Environment

## SPE

Sitecore PowerShell Extensions

# Cmdlets

## Add-ItemAcl
SPE Cmdlet - Adds new access rule to an item allowing for the item to have the access granted or denied for a provided role or user.
[appendix/commands/Test-ItemAcl.html]

## Add-ItemLanguage
SPE Cmdlet - Creates a version of the item in a new language based on an existing language version.

## Add-Rendering
SPE Cmdlet - Adds a rendering to a chosen device for the presentation of an item.

## Add-RoleMember
SPE Cmdlet - Adds one or more Sitecore users to the specified role.

## Clear-ItemAcl
SPE Cmdlet - Removes all security information from the item specified.

## Close-Window
SPE Cmdlet - Closes the runner job window after the script completes.

## ConvertFrom-CliXml
SPE Cmdlet - Imports a CliXml string with data that represents Microsoft .NET objects and creates the objects within PowerShell.

## ConvertFrom-ItemClone
SPE Cmdlet - Converts an item from a clone to a fully independent item.

## ConvertTo-Bucket
SPE Cmdlet - 
ConvertTo-Bucket [-Item] <Item> [<CommonParameters>]

## ConvertTo-CliXml
SPE Cmdlet - Exports Microsoft .NET objects froms PowerShell to a CliXml string.

## Disable-User
SPE Cmdlet - Disables the specified Sitecore user.

## Enable-User
SPE Cmdlet - Enables the specified Sitecore user.

## Expand-Token
SPE Cmdlet - Expands tokens in fields for items.

## Export-Item
SPE Cmdlet - Exports (serializes) the Sitecore item to the filesystem.

## Export-Package
SPE Cmdlet - Exports a Sitecore installation package and project.

## Export-Role
SPE Cmdlet - Exports (serializes) Sitecore roles to the filesystem on the server.

## Export-UpdatePackage
SPE Cmdlet - Exports a Sitecore update package containing a serialization diff list.

## Export-User
SPE Cmdlet - Export (serialize) a Sitecore user to the filesystem on the server.

## Find-Item
SPE Cmdlet - Finds items using the Sitecore Content Search API.

## Get-Archive
SPE Cmdlet - Returns Sitecore database archives.

## Get-Cache
SPE Cmdlet - Retrieves a Sitecore cache.

## Get-Database
SPE Cmdlet - Retrieves a Sitecore Database.

## Get-Domain
SPE Cmdlet - Gets all available domains or the specified domain.

## Get-Index
SPE Cmdlet - Returns Sitecore database indices.

## Get-ItemAcl
SPE Cmdlet - Retrieves security access rules from an item.

## Get-ItemByUri
SPE Cmdlet - This command has been obsoleted - use "Get-Item -Uri" instead.

## Get-ItemClone
SPE Cmdlet - Returns all the clones for the specified item.

## Get-ItemField
SPE Cmdlet - Retrieves item fields as either names or fields or template fields.

## Get-ItemReference
SPE Cmdlet - Returns all the items linked to the specified item..

## Get-ItemReferrer
SPE Cmdlet - Returns all the items referring to the specified item.

## Get-ItemTemplate
SPE Cmdlet - Returns the item template and its base templates.

## Get-ItemWorkflowEvent
SPE Cmdlet - Returns entries from the history store notifying of workflow state change for the specified item.

## Get-Layout
SPE Cmdlet - Returns the layout for the specified item.

## Get-LayoutDevice
SPE Cmdlet - Returns the layout for the specified device.

## Get-Package
SPE Cmdlet - Loads the package definition (xml) from the specified path.

## Get-Preset
SPE Cmdlet - Returns a serialization preset for use with Export-Item.

## Get-Rendering
SPE Cmdlet - Returns a RenderingDefinition for an item using the filtering parameters.

## Get-Role
SPE Cmdlet - Returns one or more Sitecore roles using the specified criteria.

## Get-RoleMember
SPE Cmdlet - Returns the Sitecore users in the specified role.

## Get-ScriptSession
SPE Cmdlet - Returns the list of PowerShell Extension Sessions running in the background.

## Get-SearchIndex
SPE Cmdlet - Returns sitecore Search indices.

## Get-Session
SPE Cmdlet - Returns one or more Sitecore user sessions using the specified criteria.

## Get-SpeModule
SPE Cmdlet - Returns the object that describes a Sitecore PowerShell Extensions Module

## Get-SpeModuleFeatureRoot
SPE Cmdlet - Returns the library item or path to the library where scripts for a particular integration point should be located for a specific module.

## Get-TaskSchedule
SPE Cmdlet - Returns one or more task schedule items using the specified criteria.

## Get-UpdatePackageDiff
SPE Cmdlet - Performs a diff operation between the Source and taget path akin to Sitecore Courier. The diff is the difference that takes the content of Source folder and transforms it to Target.
IMPORTANT! This fun
ctionality requires changes to web.config file on your sitecore server to work. Please consult the first Example.

## Get-User
SPE Cmdlet - Returns one or more Sitecore users using the specified criteria.

## Get-UserAgent
SPE Cmdlet - Returns the current user's browser user agent.

## Import-Function
SPE Cmdlet - Imports a function script from the script library's "Functions" folder.

## Import-Item
SPE Cmdlet - Imports (deserializes) the specified path from the filesystem on the server as a Sitecore item.

## Import-Role
SPE Cmdlet - Imports (deserializes) Sitecore roles from the Sitecore server filesystem.

## Import-User
SPE Cmdlet - Imports (deserializes) Sitecore users from the Sitecore server filesystem.

## Initialize-Item
SPE Cmdlet - Initializes items with the PowerShell automatic properties for each field.

## Install-Package
SPE Cmdlet - Installs a Sitecore package from the specified path.

## Install-UpdatePackage
SPE Cmdlet - Installs a Sitecore update package from the specified path.

## Invoke-Script
SPE Cmdlet - Executes a script from Sitecore PowerShell Extensions Script Library.
This command used to be named Execute-Script - a matching alias added for compatibility with older scripts.

## Invoke-ShellCommand
SPE Cmdlet - Executes Sitecore Shell command for an item.
This command used to be named Execute-ShellCommand - a matching alias added for compatibility with older scripts.

## Invoke-Workflow
SPE Cmdlet - Executes Workflow action for an item.
This command used to be named Execute-Workflow - a matching alias added for compatibility with older scripts.

## Lock-Item
SPE Cmdlet - Locks the Sitecore item by the current or specified user.

## Login-User
SPE Cmdlet - Logs a user in and performs further script instructions in the context of the user.

## Logout-User
SPE Cmdlet - Logs the current user out.

## New-Domain
SPE Cmdlet - Creates a new domain with the specified name.

## New-ExplicitFileSource
SPE Cmdlet - Creates new File source that can be added to a Sitecore package.

## New-ExplicitItemSource
SPE Cmdlet - Creates new Explicit Item Source that can be added to a Sitecore package.

## New-FileSource
SPE Cmdlet - Creates new File source that can be added to a Sitecore package.

## New-ItemAcl
SPE Cmdlet - Creates a new access rule for use with Set-ItemAcl and Add-ItemAcl cmdlets.

## New-ItemClone
SPE Cmdlet - Creates a new item clone based on the item provided.

## New-ItemSource
SPE Cmdlet - Creates new Item source that can be added to a Sitecore package.

## New-ItemWorkflowEvent
SPE Cmdlet - Creates new entry in the history store notifying of workflow state change.

## New-Package
SPE Cmdlet - Creates a new Sitecore install package object.

## New-Rendering
SPE Cmdlet - Creates new rendering definition that can later be added to an item.

## New-SecuritySource
SPE Cmdlet - Creates new User & Role source that can be added to a Sitecore package.

## New-User
SPE Cmdlet - Creates a new Sitecore user.

## Protect-Item
SPE Cmdlet - Protects the Sitecore item.

## Publish-Item
SPE Cmdlet - Publishes a Sitecore item.

## Read-Variable
SPE Cmdlet - Prompts user to provide values for variables required by the script to perform its operation.

## Receive-File
SPE Cmdlet - Shows a dialog to users allowing to upload files to either server file system or items in media library.

## Remove-Domain
SPE Cmdlet - Removes the specified domain.

## Remove-ItemLanguage
SPE Cmdlet - Removes Language from a single item or a branch of items

## Remove-Rendering
SPE Cmdlet - Removes renderings from an item.

## Remove-RoleMember
SPE Cmdlet - Removes one or more Sitecore users from the specified role.

## Remove-ScriptSession
SPE Cmdlet - Removes a persistent Script Session from memory.

## Remove-Session
SPE Cmdlet - Removes one or more Sitecore user sessions.

## Remove-User
SPE Cmdlet - Removes the Sitecore user.

## Restart-Application
SPE Cmdlet - Restarts the Sitecore Application pool.

## Send-File
SPE Cmdlet - Allows users to download files from server and file items from media library.

## Send-SheerMessage
SPE Cmdlet - Sends a sheer message to the app in which context the script is executing.

## Set-HostProperty
SPE Cmdlet - Sets the current host property.

## Set-ItemAcl
SPE Cmdlet - Sets new security information on an item overwriting the previous settings.

## Set-Layout
SPE Cmdlet - Sets item layout for a device.

## Set-Rendering
SPE Cmdlet - Updates rendering with new values.

## Set-User
SPE Cmdlet - Sets the Sitecore user properties.

## Set-UserPassword
SPE Cmdlet - Sets the Sitecore user password.

## Show-Alert
SPE Cmdlet - Pauses the script and shows an alert to the user.

## Show-Application
SPE Cmdlet - Executes Sitecore Sheer application.

## Show-Confirm
SPE Cmdlet - Shows a user a confirmation request message box.

## Show-FieldEditor
SPE Cmdlet - Shows Field editor for a provided item.

## Show-Input
SPE Cmdlet - Shows prompt message box asking user to provide a text string.

## Show-ListView
SPE Cmdlet - Sends output to an interactive table in a separate window.

## Show-ModalDialog
SPE Cmdlet - Shows Sitecore Sheer control as a modal dialog.

## Show-Result
SPE Cmdlet - Shows a Sheer dialog with text results showing the output of the script or another control selected by the user based on either control name or Url to the control.

## Show-YesNoCancel
SPE Cmdlet - Shows Yes/No/Cancel dialog to the user and returns user choice.

## Start-TaskSchedule
SPE Cmdlet - Executes a task schedule.

## Test-ItemAcl
SPE Cmdlet - Tests a specific access right for a specified user against the provided item

## Test-Rule
SPE Cmdlet - Tests item against a sitecore serialized rules engine rule set.

## Unlock-Item
SPE Cmdlet - Unlocks the specified Sitecore item.

## Unlock-User
SPE Cmdlet - Unlocks a Sitecore user using the specified criteria.

## Unprotect-Item
SPE Cmdlet - Unprotects the specified Sitecore item.

## Update-ListView
SPE Cmdlet - Updates List View (created by Show-ListView) data.

## Write-Log
SPE Cmdlet - Writes text to the Sitecore event log.