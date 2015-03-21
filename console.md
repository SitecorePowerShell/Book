# Console

The SPE Console is a command line interface (CLI) that provides a streamlined tool to working with Windows PowersShell and Sitecore.

Here's a quick look at the Console:
![Console](Console-Empty.png)

A few things to note about the console:
* You can interact with the providers available in the standard Windows PowerShell Console. Below are some of the important providers.Run the command *Get-PSProvider* to see the complete list.
 * FileSystem - Supports interacting with files and folders.
 * CmsItemProvider - Supports interacting with the Sitecore content items.
* The console prompt typically begins with *PS master:\\>* which indicates that the present working directory is using the *CmsItemProvider* and set to the *master* database. Some examples to change directories between providers:

 ```powershell
 PS master:\> cd core:
 PS core:\> cd C:
 PS C:\> Set-Location -Path master:
 ```
 **Note:** The command *cd* is an alias for *Set-Location*.
 
* The SPE module provides some convenient variables out of the box for use in running commands and scripts. The variables prefixed with *Sitecore* are pulled from the Web.config settings. Run the command *Get-Variable* to see the complete list.
 * AppPath - Path to the installed directory for Sitecore.
 * Me - Current logged in user for Sitecore.
 * PWD - Present working directory for the console.
 * SitecoreDataFolder
 * SitecoreDebugFolder
 * SitecoreIndexFolder
 * SitecoreLayoutFolder
 * SitecoreLogFolder
 * SitecoreMediaFolder
 * SitecorePackageFolder
 * SitecoreSerializationFolder
 * SitecoreTempFolder

 **Note:** Any new variables created in the console are stored within the session of each console instance; when the session ends the variables are removed.