# Integration Points

The SPE Modules support a wide variety of predefined script libraries to help with exposing scripts to the Sitecore interface.

The following list outlines the available libraries for _Integration Points_ that may be added to modules. These may also be generated automatically when creating or updating modules.

{% hint style="info" %}
Be certain to enable the module when you need to use it.
{% endhint %}

* [**Content Editor**](content-editor.md)
  * Context Menu - Visibility can be control using rules made available from within the _Interactive_ field section.
  * Gutters - Requires the library to be synced.
  * Insert Item - Visibility can be control using rules made available from within the _Interactive_ field section.
  * Ribbon - Requires the library to be synced. Visibility can be control using rules made available from within the _Interactive_ field section.
  * Warning - Appears as a warning message beneath the ribbon in the _Content Editor_.
* [**Control Panel**](control-panel.md) - Requires the library to be synced.
* [**Data Sources**](https://github.com/sitecorepowershell/sitecore-powershell-extensions/tree/b6365f8ecf54966c2e1757f549f543976500aa52/date-sources.md) - Not a formal integration point library, but a good place to add scripts used for Data Source Scripts. Read more about how they work [here](https://blog.najmanowicz.com/2013/04/17/powershell-scripted-datasources-in-sitecore-part-1/) and [here](https://github.com/SitecorePowerShell/Book/tree/9c7126d7a38df6ef372e8baef52f9a02baabd550/modules/integration-points/[https:/blog.najmanowicz.com/2013/05/06/powershell-scripted-data-sources-in-sitecore-part-2/]/README.md). The _Source_ field simply references the script like _script:/sitecore/system/Modules/PowerShell/Script Library/\[MODULE NAME\]/Data Sources/\[SCRIPT NAME\]_.
* [**Event Handlers**](event-handlers.md) - Requires a patch file to enable the desired events. Read more about it [here](https://blog.najmanowicz.com/2013/05/27/react-to-sitecore-events-with-powershell-scripts/).
* [**Functions**](functions.md) - Exposes scripts to the command _Import-Function_.
* **Internal**
  * [ISE Plugins](ise-plugins.md) - Appears as a button in the ISE _Settings_ tab.
  * List View - Represents the ribbon buttons such as for **Actions** and **Exports**.
* [**Page Editor**](page-editor.md) \(Experience Editor\)
  * Notification - Appears as a notification message beneath the ribbon in the _Experience Editor_.
* [**Pipelines**](pipelines.md)
  * LoggedIn
  * LoggingIn
  * Logout
* [**Reports**](reports/) - Appears as a shortcut under _Sitecore -&gt; Reporting Tools -&gt; PowerShell Reports_.
* [**Toolbox**](toolbox.md) - Appears as a shortcut under _Sitecore -&gt; Toolbox_. Visibility can be control using rules made available from within the _Interactive_ field section.
* [**Tasks**](tasks/) - Not a formal integration point library, but a good place to add scripts used for Scheduled Tasks. SPE includes a `PowerShellScriptCommand` found under _/sitecore/system/Tasks/Commands/PowerShellScriptCommand_ which is used to run scripts from within scheduled tasks.
* [**Web API**](web-api.md) - Exposes scripts than can be consumed through [SPE Remoting](../../remoting.md). The script can be executed by requesting a specific url.
* [**Workflows**](workflows.md) - Not a formal integration point library, but a good place to add scripts used for Workflow Action Scripts. Read more about how they work [here](https://blog.najmanowicz.com/2014/11/09/introducing-powershell-actions-for-sitecore-workflows/).

## Other Integrations

* **Remoting** - Interact with SPE through the provided web services as described [here](../../remoting.md). A Windows PowerShell module is made available to encapsulate the web service calls into PowerShell commands.

