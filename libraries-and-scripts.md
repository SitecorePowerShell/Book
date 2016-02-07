# Libraries and Scripts

Modules may contain *PowerShell Script Library* items and *PowerShell Script* items. The following section outlines some of the basic concepts you need to know for the following chapters.

### PowerShell Script Library

The library items represent a collection of scripts, and may be structured with one or more levels of libraries. 

#### Naming Convention 

You'll find that with the *Integration Points* some libraries should be created with specific names (i.e. Content Editor, Control Panel). 

As a best practice we recommend that the *Function* library  consist of reusable functions while other libraries contain the solution specific scripts. 

**Example:** The following demonstrates the use of the *Function* script library containing *Get-DateMessage*.

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

Some names we've used in the module include:
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

#### Fields

**Interactive** : The following fields support the two custom rules as well as a variety of out-of-the-box rules.

* **ShowRule** (Show if rules are met or not defined)
 * **PowerShell**
   * where *specific* persistent PowerShell session was already initiated
   * where *specific* persistent PowerShell session was already initiated and has the *specific* variable defined and not null
   * where exposed in a *specific* view
 * **PowerShell ISE**
   * when *length* script length is *compares to* *number* characters long
   * when the edited script is *in a state*
* **EnableRule** (Enable if rules are met or not defined)
  * ***Same as above*** 

### PowerShell Script

The script items represent the code that will be executed.

#### Naming Convention

There are three conventions that we recommend you follow which are shown with an example below.

* **Title Casing** - This should be used when the name will be exposed in places such as the *Content Editor*, script library names, and *Reports* root directory.
* **Sentence casing** - This should be used when the name is long and will not be visible to the user or is a report with a very long name.
* **Noun-Verb** - This should be used when the script is stored within the *Functions* script library and will be imported using the command *Import-Function*.

#### Fields

**Interactive** : Refer to the description shown for *PowerShell Script Library* fields.

**Scripting**

* **Script** (Script body) : This is a multi-line text than should be edited using the **PowerShell ISE** application. 

**Session Persistency**

* **PersistentSessionId** (Persistent Session ID) : Context scripts using this ID will execute in a single session and be reused; leaving empty will cause the session to be discarded after execution. This value should be used for rules requesting the session ID.

