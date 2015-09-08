# Contributor Guide

The following guide should provide you with enough information to setup a development environment configured to contribute to SPE. We'll begin with a single installation of Sitecore 8+. Adam wrote an [article][1] (written March 2015) that goes into great detail about the installation steps. We've made some changes along the way and as you would expect it's made its way into this book.

The solution is upgraded to Visual Studio 2015.

#### Single Instance

1. Meet the prerequisites found [here](installation.md)
2. Install Sitecore 8+ to `C:\inetpub\wwwroot\Console`
3. Fetch source code to installation path
```
 cd "C:\inetpub\wwwroot\Console"
 git init
 git remote add origin https://github.com/SitecorePowerShell/Console.git
 git fetch origin
 git checkout -b master --track origin/master
 ```
4. Copy `C:\inetpub\wwwroot\Console\deploy.targets.sample` to `C:\inetpub\wwwroot\Console\deploy.targets`
5. Edit the paths in `deploy.targets` with the appropriate *SitecorePath* and *LibrariesPath*
6. Copy Sitecore dependencies to `C:\inetpub\wwwroot\Console\Libraries`
 - Sitecore.Analytics.dll
 - Sitecore.Client.dll
 - Sitecore.ContentSearch.dll
 - Sitecore.ContentSearch.Linq.dll
 - Sitecore.ExperienceEditor.dll
 - Sitecore.Kernel.dll
 - Sitecore.Logging.dll
 - Sitecore.NVelocity.dll
 - Sitecore.Update.dll
7. Run the Windows PowerShell script as an Administrator 
```powershell
. C:\inetpub\wwwroot\Console\Setup-Folders.ps1
```
8. Compile the solution in Visual Studio
 - Because SPE supports Sitecore 7+, you'll need to unload the `Cognfide.PowerShell.Sitecore7` project or setup the reference path for Sitecore 7.x libraries. We'll see that later on in this tutorial.
9. Sync Sitecore items with the Windows PowerShell script as an Administrator
```
. C:\inetpub\wwwroot\Console\Setup-Module.ps1
```
10. Navigate to http://console/sitecore/login and verify that SPE works as expected.

#### Multiple Instance

1. Meet the prerequisites found [here](installation.md)
2. Install Sitecore 7+ to the following paths    
 - `C:\inetpub\wwwroot\Sitecore8`
 - `C:\inetpub\wwwroot\Sitecore75`
 - `C:\inetpub\wwwroot\Sitecore70`
3. Fetch source code to installation path
```
 cd "C:\Projects\SitecorePowerShell\"
 git init
 git remote add origin https://github.com/SitecorePowerShell/Console.git
 git fetch origin
 git checkout -b master --track origin/master
 ```
4. Copy `C:\Projects\SitecorePowerShell\deploy.targets.sample` to `C:\Projects\SitecorePowerShell\deploy.targets`
5. Edit the paths in `deploy.targets` with the appropriate *SitecorePath* and *LibrariesPath*
6. Copy Sitecore dependencies to `C:\Projects\SitecorePowerShell\Libraries` with a subfolder for each Sitecore version (i.e. \SC8, \SC75, \SC7). Use the reference path in Visual Studio to match the right assembly versions in `Cognifide.PowerShell.Sitecore7` and `Cognifide.PowerShell.Sitecore8`. Not doing so will result in error messages related to those projects.
 - Sitecore.Analytics.dll
 - Sitecore.Client.dll
 - Sitecore.ContentSearch.dll
 - Sitecore.ContentSearch.Linq.dll
 - Sitecore.ExperienceEditor.dll
 - Sitecore.Kernel.dll
 - Sitecore.Logging.dll
 - Sitecore.NVelocity.dll
 - Sitecore.Update.dll
7. Run the Windows PowerShell script as an Administrator. Modifications to the script may be necessary to support your installation paths.
```powershell
. C:\inetpub\wwwroot\Console\Setup-Folders.ps1
```
8. Compile the solution in Visual Studio
9. Sync Sitecore items with the Windows PowerShell script as an Administrator. Modifications to the script may be necessary to support your installation urls.
```
. C:\inetpub\wwwroot\Console\Setup-Module.ps1
```
10. Navigate to each of your installation urls and verify that SPE works as expected.

[1]: http://blog.najmanowicz.com/2015/03/03/set-up-sitecore-powershell-extensions-development-environment/ 
