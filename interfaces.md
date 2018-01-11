# Interfaces

Here we'll discuss the Console, ISE, and other dialogs made available through SPE.

[![The Sitecore PowerShell Console is a command line interface that many power users find great for quickly running commands.](images/screenshots/cli-empty.png)](https://youtu.be/1TLYyzTw01w "Click for a quick demo")

[![The Sitecore PowerShell ISE is a scripting interface for running commands and authoring scripts.](images/screenshots/ise-empty.png)](http://youtu.be/RCDprfRsbSU "Click for a quick demo")

### Providers
You can interact with the providers typically available in the standard Windows PowerShell Console. Below are some of the important providers. Run the command ` Get-PSProvider ` to see the complete list.
 * **FileSystem** - Supports interacting with files and folders.
 * **CmsItemProvider** - Supports interacting with the Sitecore content items.

The console prompt typically begins with ` PS master:\> `. The present working directory is using the *CmsItemProvider* and set to the *master* database. 
 
 **Example:** Change directories between providers.
 ```powershell
 PS master:\> cd core:
 PS core:\> cd C:
 PS C:\windows\system32\inetsrv> Set-Location -Path master:
 PS master:\>
 ```
 **Note:** Include the backslash in the path (i.e. **C:\\**) to get the root of the drive when interacting with the *FileSystem* provider; the behavior seen is different than you might expeect because of *w3wp.exe*. See issue #[314][1].

### Variables

Read more about the different variables [here](/variables.md).