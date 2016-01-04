# Libraries and Scripts

Modules may contain *PowerShell Script Library* items and *PowerShell Script* items. The following section outlines some of the basic concepts you need to know for the following chapters.

### PowerShell Script Library

The library items represent a collection of scripts, and may be structured with one or more levels of libraries. 

#### Naming Convention 

You'll find that with the *Integration Points* some libraries should be created with specific names (i.e. Content Editor, Control Panel).

#### Rules

The library item contains a field section called *Interactive* which provides the two available types of rule fields.

**Fields:** The following fields support the two custom rules as well as a variety of out-of-the-box rules.
* Show if rules are met or not defined
 * **Rule:** PowerShell
   * where *specific* persistent PowerShell session was already initiated
   * where *specific* persistent PowerShell session was already initiated and has the *specific* variable defined and not null
   * where exposed in a *specific* view
 * **Rule:** PowerShell ISE
   * when *length* script length is *compares to* *number* characters long
   * when the edited script is *in a state*
* Enable if rules are met or not defined
  * *Same as above* 
