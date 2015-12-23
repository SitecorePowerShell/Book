# Integration Points

The SPE Modules support a wide variety of predefined script libraries to help with exposing scripts to the Sitecore interface.

The following list outlines the available libraries for *Integration Points*. These may also be generated automatically when creating or updating modules.

* Content Editor
 * Context Menu - Item visibility can be control using rules made available from within the *Interactive* field section.
 * Gutters - Requires the library to be synced.
 * Insert Item - Item visibility can be control using rules made available from within the *Interactive* field section.
 * Ribbon - Requires the library to be synced. Item visibility can be control using rules made available from within the *Interactive* field section.
 * Warning - Appears as a warning message beneath the ribbon in the *Content Editor*.
* Control Panel - Requires the library to be synced.
* Event Handlers - Requires a patch file to enable the desired events.
* Functions - Exposes scripts to the command *Import-Function*.
* Internal
 * ISE Plugins - Appears as a button in the ISE *Settings* tab.
 * List View - Represents the ribbon buttons such as for **Actions** and **Exports**.
* Page Editor (Experience Editor)
 * Notification - Appears as a notification message beneath the ribbon in the *Experience Editor*.
* Pipelines
 * LoggedIn
 * LoggingIn
 * Logout
* Reports - Appears as a shortcut under *Sitecore -> Reporting Tools -> PowerShell Reports*.
* Toolbox - Appears as a shortcut under *Sitecore -> Toolbox*. Item visibility can be control using rules made available from within the *Interactive* field section.
* Tasks - Not a formal integration point, but a good place to add scripts used for Scheduled Tasks.
* Web API - Exposes scripts through [SPE Remoting](remoting.md).

