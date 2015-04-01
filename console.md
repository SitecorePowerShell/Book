# Console

The SPE Console is a command line interface (CLI) designed for efficiency. The console provides a streamlined tool for working within Windows PowersShell and Sitecore.

Here's a quick look at the Console:
![Console](images/screenshots/cli-empty.png)

You can interact with the providers available in the standard Windows PowerShell Console. Below are some of the important providers.Run the command *Get-PSProvider* to see the complete list.
 * FileSystem - Supports interacting with files and folders.
 * CmsItemProvider - Supports interacting with the Sitecore content items.

The console prompt typically begins with *PS master:\\>*. The present working directory is using the *CmsItemProvider* and set to the *master* database. 
 
 **Example:** Change directories between providers.
 ```powershell
 PS master:\> cd core:
 PS core:\> cd C:
 PS C:\windows\system32\inetsrv> Set-Location -Path master:
 PS master:\>
 ```
 **Note:** Include the backslash in the path (i.e. **C:\\**) to get the root of the drive when interacting with the *FileSystem* provider; the behavior seen is different than you might expeect because of *w3wp.exe*. See issue #[314][1].
 
SPE provides some convenient variables out of the box for use in running commands and scripts. The variables prefixed with *Sitecore* derive from the Web.config settings. Run the command *Get-Variable* to see the complete list.

| Variable | Example |
| -------- | ----------- |
| AppPath  | C:\Inetpub\wwwroot\Console\Website\ |
| Me        | sitecore\admin    |
| PWD       | master:\     |
| SitecoreDataFolder    | C:\Inetpub\wwwroot\Console\Data\    |
| SitecoreDebugFolder   | C:\Inetpub\wwwroot\Console\Data\/debug  |
| SitecoreIndexFolder   | C:\Inetpub\wwwroot\Console\Data\/indexes  |
| SitecoreLayoutFolder  | C:\Inetpub\wwwroot\Console\Website\layouts  |
| SitecoreLogFolder     | C:\Inetpub\wwwroot\Console\Data\/logs  |
| SitecoreMediaFolder   | C:\Inetpub\wwwroot\Console\Website\upload  |
| SitecorePackageFolder | C:\Inetpub\wwwroot\Console\Data\/packages  |
| SitecoreSerializationFolder   | C:\Inetpub\wwwroot\Console\Data\/serialization  |
| SitecoreTempFolder    | /temp  |

 **Note:** Any new variables created are stored within the session of console instance; when the session ends the variables are removed.

[1]: https://github.com/SitecorePowerShell/Console/issues/314