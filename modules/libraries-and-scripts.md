# Libraries and Scripts

Modules may contain _PowerShell Script Library_ items and _PowerShell Script_ items. The following section outlines some of the basic concepts you need to know for the following chapters.

## PowerShell Script Library

The library items represent a collection of scripts, and may be structured with one or more levels of libraries.

### Naming Convention

You'll find that with the _Integration Points_ some libraries should be created with specific names \(i.e. Content Editor, Control Panel\).

As a best practice we recommend that the _Function_ library consist of reusable functions while other libraries contain the solution specific scripts.

**Example:** The following demonstrates the use of the _Function_ script library containing _Get-DateMessage_.

```text
# /sitecore/system/modules/powershell/script library/spe rocks/functions/get-datemessage
function Get-DateMessage {
  "The current date and time is: $(Get-Date)"
}
```

```text
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

Many of the of the libraries are integration points for the module.

### Fields

**Interactive** : The following fields support the two custom rules as well as a variety of out-of-the-box rules.

* **ShowRule** \(Show if rules are met or not defined\)
  * **PowerShell**
    * where _specific_ persistent PowerShell session was already initiated
    * where _specific_ persistent PowerShell session was already initiated and has the _specific_ variable defined and not null
    * where exposed in a _specific_ view
  * **PowerShell ISE**
    * when _length_ script length is _compares to_ _number_ characters long
    * when the edited script is _in a state_
* **EnableRule** \(Enable if rules are met or not defined\)
  * _**Same as above**_ 

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

