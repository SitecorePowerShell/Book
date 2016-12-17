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
SPE provides some convenient variables out of the box for use in running commands and scripts. The variables prefixed with *Sitecore* derive from the Web.config settings. Run the command ` Get-Variable ` to see the complete list.

| **Variable** | **Example** |
| -------- | ----------- |
| AppPath  | C:\Inetpub\wwwroot\Console\Website\ |
| HostSettings | ... <br/> FontSize : 12 <br/> FontFamily : Wingdings <br/> ... |
| me        | sitecore\admin    |
| PWD       | master:\     |
| ScriptSession | ... <br/> ID : e9fedd64-cad0-4c43-b950-9cd361b151fd <br/> ... |
| SitecoreAuthority | https://console |
| SitecoreDataFolder    | C:\Inetpub\wwwroot\Console\Data    |
| SitecoreDebugFolder   | C:\Inetpub\wwwroot\Console\Data\debug  |
| SitecoreIndexFolder   | C:\Inetpub\wwwroot\Console\Data\indexes  |
| SitecoreLayoutFolder  | C:\Inetpub\wwwroot\Console\Website\layouts  |
| SitecoreLogFolder     | C:\Inetpub\wwwroot\Console\Data\logs  |
| SitecoreMediaFolder   | C:\Inetpub\wwwroot\Console\Website\upload  |
| SitecorePackageFolder | C:\Inetpub\wwwroot\Console\Data\packages  |
| SitecoreScriptRoot | master:\system\Modules\PowerShell\Script Library\Task Management\Toolbox |
| SitecoreCommandPath | master:\system\Modules\PowerShell\Script Library\Task Management\Toolbox\Task Manager |
| SitecoreSerializationFolder   | C:\Inetpub\wwwroot\Console\Data\serialization  |
| SitecoreTempFolder    | C:\Inetpub\wwwroot\Console\Website\temp  |
| SitecoreVersion | 8.2.160729 |

 **Note:** Any new variables created are stored within the session of console instance; when the session ends the variables are removed. You'll also need to be careful not to overwrite the built-in variables.
