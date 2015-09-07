# Contributor Guide

The following guide should provide you with enough information to setup a development environment configured to contribute to SPE. We'll begin with a single installation of Sitecore 8+.

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