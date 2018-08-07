# Variables

SPE provides some convenient variables out of the box for use in running commands and scripts. Many of the variables prefixed with _Sitecore_ derive from the Sitecore.config settings. Run the command `Get-Variable` to see the complete list.

| **Variable** | **Example** |
| :--- | :--- |
| AppPath | C:\Inetpub\wwwroot\Console\Website\ |
| HostSettings | ...   FontSize : 12   FontFamily : Wingdings   ... |
| me | sitecore\admin |
| PWD | master:\ |
| PSScript | `$PSScript.Appearance.Icon # Returns the icon of the executing script` |
| ScriptSession | ...   ID : e9fedd64-cad0-4c43-b950-9cd361b151fd   ... |
| SitecoreAuthority | [https://console](https://console) |
| SitecoreContextItem | `$SitecoreContextItem.Language.Name # Returns the language name` |
| SitecoreDataFolder | C:\Inetpub\wwwroot\Console\Data |
| SitecoreDebugFolder | C:\Inetpub\wwwroot\Console\Data\debug |
| SitecoreIndexFolder | C:\Inetpub\wwwroot\Console\Data\indexes |
| SitecoreLayoutFolder | C:\Inetpub\wwwroot\Console\Website\layouts |
| SitecoreLogFolder | C:\Inetpub\wwwroot\Console\Data\logs |
| SitecoreMediaFolder | C:\Inetpub\wwwroot\Console\Website\upload |
| SitecorePackageFolder | C:\Inetpub\wwwroot\Console\Data\packages |
| SitecoreScriptRoot | master:\system\Modules\PowerShell\Script Library\Task Management\Toolbox |
| SitecoreCommandPath | master:\system\Modules\PowerShell\Script Library\Task Management\Toolbox\Task Manager |
| SitecoreSerializationFolder | C:\Inetpub\wwwroot\Console\Data\serialization |
| SitecoreTempFolder | C:\Inetpub\wwwroot\Console\Website\temp |
| SitecoreVersion | 8.2.160729 |

**Note:** Any new variables created are stored within the session of console instance; when the session ends the variables are removed. You'll also need to be careful not to overwrite the built-in variables.

