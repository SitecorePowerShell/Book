# Contributor Guide

High level steps:
1. Install Sitecore instance here C:\inetpub\wwwroot\Sitecore8\ 
2. Clone source code here C:\Projects\SitecorePowerShell\
3. Install latest version of SPE found here C:\Projects\SitecorePowerShell\Data\packages
4. Execute C:\Projects\SitecorePowerShell\Setup-Folders.ps1
5. Revert Sitecore trees to the latest items
6. Setup Sitecore libraries
7. Add post-build setup
8. Serialize item changes

The following guide should provide you with enough information to setup a development environment configured to contribute to SPE. We'll begin with a single installation of Sitecore 8+.

The solution is upgraded to Visual Studio 2015.

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
4. Copy Sitecore dependencies
 - Sitecore.Analytics.dll
 - Sitecore.Client.dll
 - Sitecore.ContentSearch.dll
 - Sitecore.ContentSearch.Linq.dll
 - Sitecore.ExperienceEditor.dll
 - Sitecore.Kernel.dll
 - Sitecore.Logging.dll
 - Sitecore.NVelocity.dll
 - Sitecore.Update.dll


