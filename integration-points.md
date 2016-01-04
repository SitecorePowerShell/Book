# Integration Points

The SPE Modules support a wide variety of predefined script libraries to help with exposing scripts to the Sitecore interface.

The following list outlines the available libraries for *Integration Points* that may be added to modules. These may also be generated automatically when creating or updating modules. 

**Note:** Be certain to enable the module when you need to use it.

* **[Content Editor](content-editor.md)**
 * Context Menu - Visibility can be control using rules made available from within the *Interactive* field section.
 * Gutters - Requires the library to be synced.
 * Insert Item - Visibility can be control using rules made available from within the *Interactive* field section.
 * Ribbon - Requires the library to be synced. Visibility can be control using rules made available from within the *Interactive* field section.
 * Warning - Appears as a warning message beneath the ribbon in the *Content Editor*.
* **Data Sources** - Not a formal integration point library, but a good place to add scripts used for Data Source Scripts. Read more about how they work [here][3] and [here][4]. The *Source* field simply references the script like *script:/sitecore/system/Modules/PowerShell/Script Library/[MODULE NAME]/Data Sources/[SCRIPT NAME]*.
* **Control Panel** - Requires the library to be synced.
* **Event Handlers** - Requires a patch file to enable the desired events. Read more about it [here][2].
* **Functions** - Exposes scripts to the command *Import-Function*.
* **Internal**
 * ISE Plugins - Appears as a button in the ISE *Settings* tab.
 * List View - Represents the ribbon buttons such as for **Actions** and **Exports**.
* **Page Editor** (Experience Editor)
 * Notification - Appears as a notification message beneath the ribbon in the *Experience Editor*.
* **Pipelines**
 * LoggedIn
 * LoggingIn
 * Logout
* **[Reports](reports.md)** - Appears as a shortcut under *Sitecore -> Reporting Tools -> PowerShell Reports*.
* **[Toolbox](toolbox.md)** - Appears as a shortcut under *Sitecore -> Toolbox*. Visibility can be control using rules made available from within the *Interactive* field section.
* **[Tasks](tasks.md)** - Not a formal integration point library, but a good place to add scripts used for Scheduled Tasks. SPE includes a `PowerShellScriptCommand` found under */sitecore/system/Tasks/Commands/PowerShellScriptCommand* which is used to run scripts from within scheduled tasks.
* **[Web API](web-api.md)** - Exposes scripts than can be consumed through [SPE Remoting](remoting.md). The script can be executed by requesting a specific url.
* **Workflows** - Not a formal integration point library, but a good place to add scripts used for Workflow Action Scripts. Read more about how they work [here][1].

### Other Integrations

* **Remoting** - Interact with SPE through the provided web services as described [here](remoting.md). A Windows PowerShell module is made available to encapsulate the web service calls into PowerShell commands.

[1]: http://blog.najmanowicz.com/2014/11/09/introducing-powershell-actions-for-sitecore-workflows/
[2]: http://blog.najmanowicz.com/2013/05/27/react-to-sitecore-events-with-powershell-scripts/
[3]: http://blog.najmanowicz.com/2013/04/17/powershell-scripted-datasources-in-sitecore-part-1/
[4]: http://blog.najmanowicz.com/2013/05/06/powershell-scripted-data-sources-in-sitecore-part-2/