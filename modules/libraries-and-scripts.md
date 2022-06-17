# Libraries and Scripts

Modules may contain _PowerShell Script Library_ items and _PowerShell Script_ items. The following section outlines some of the basic concepts you need to know for the following chapters.

## PowerShell Script Library

The library items represent a collection of scripts, and may be structured with one or more levels of libraries.

### Naming Convention

You'll find that with the _Integration Points_ some libraries should be created with specific names \(i.e. Content Editor, Control Panel\).

As a best practice we recommend that the _Functions_ library consist of reusable scripts containing PowerShell functions \(i.e. `Do-Something`\) while other libraries contain the solution specific scripts \(i.e. `MakeScriptingGreatAgain`\).

**Example:** The following demonstrates the use of the _Functions_ script library containing _Get-DateMessage_.

```powershell
# /sitecore/system/modules/powershell/script library/spe rocks/functions/get-datemessage
function Get-DateMessage {
  "The current date and time is: $(Get-Date)"
}
```

```powershell
# /sitecore/system/modules/powershell/script library/spe rocks/alerts/show-datemessage
Import-Function -Name Get-DateMessage

Show-Alert (Get-DateMessage)
```

Some names we've used included:

* Beginner Tutorials
* **Content Editor**
* Content Maintenance
* Content Interrogation
* Development
* **Event Handlers**
* **Functions**
* **Internal**
* **Page Editor**
* **Pipelines**
* Profile and Security
* **Reports**
* Script Testing
* **Tasks**
* **Toolbox**
* User Interaction
* **Web API**

{% hint style="info" %}
Many of the libraries are integration points for the module. When the integration point wizard runs, you will see that these can be generated automatically.
{% endhint %}

### Fields

**Interactive** : The following fields support the two custom rules as well as a variety of out-of-the-box rules.

* **ShowRule** \(Show if rules are met or not defined\) - typically controls visibility of integration points.
  * **PowerShell**
    * where _specific_ persistent PowerShell session was already initiated
    * where _specific_ persistent PowerShell session was already initiated and has the _specific_ variable defined and not null
    * where exposed in a _specific_ view
  * **PowerShell ISE**
    * when _length_ script length is _compares to_ _number_ characters long
    * when the edited script is _in a state_
* **EnableRule** \(Enable if rules are met or not defined\) - typically controls enabled state of integration points.
  * _**Same as above**_ 

#### Rules Usage

There are a number of use cases for the **EnableRule** and **ShowRule**. 

* If there is a UI component the **ShowRule** can be used to ensure it appears while the **EnableRule** can toggle when it can be clicked.
* If there is no UI component, the **EnableRule** is used to determine when the script should be executed; useful to limit creation of PowerShell runspaces.

| ShowRule | EnableRule |
| :--- | :--- |
| Content Editor [Context Menu](integration-points/content-editor.md#context-menu) | Content Editor [Context Menu](integration-points/content-editor.md#context-menu) |
|  | Content Editor [Gutter](integration-points/content-editor.md#gutter) |
| Content Editor [Insert Item](integration-points/content-editor.md#insert-item) |  |
| Content Editor [Ribbon](integration-points/content-editor.md#ribbon) | Content Editor [Ribbon](integration-points/content-editor.md#ribbon) |
|  | Content Editor [Warning](integration-points/content-editor.md#warning) |
|  | [DataSources](integration-points/data-sources.md) |
|  | [Event Handlers](integration-points/event-handlers.md) |
| ISE Plugin | ISE Plugin |
| [Reports](integration-points/reports/) | [Reports](integration-points/reports/) |
| Reports List View Action | Reports List View Action |
| Reports List View Export | Reports List View Export |
|  | Page Editor [Notification](integration-points/page-editor.md#notification) |
|  | Page Editor [Experience Button](integration-points/page-editor.md#experience-button) |
|  | [Pipelines](integration-points/pipelines.md) |
|  | [Tasks](integration-points/tasks/) |
| [Toolbox](integration-points/toolbox.md) |  |
|  | [Workflow Action](integration-points/workflows.md) |

## PowerShell Script

The script items represent the code that will be executed.

### Naming Convention

There are three conventions that we recommend you follow which are shown with an example below.

* **Title Casing** - This should be used when the name will be exposed in places such as the _Content Editor_, script library names, and _Reports_ root directory.
* **Sentence casing** - This should be used when the name is long and will not be visible to the user or is a report with a very long name.
* **Noun-Verb** - This should be used when the script is stored within the _Functions_ script library and will be imported using the command _Import-Function_.

### Fields

**Interactive** : Refer to the description shown for _PowerShell Script Library_ [fields](libraries-and-scripts.md#fields).

**Scripting**

* **Script** \(Script body\) : This is a multi-line text than should be edited using the **PowerShell ISE** application. 

**Session Persistency**

* **PersistentSessionId** \(Persistent Session ID\) : Context scripts using this ID will execute in a single session and be reused; leaving empty will cause the session to be discarded after execution. This value should be used for rules requesting the session ID.

