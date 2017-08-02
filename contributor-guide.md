# Contributor Guide

The following guide should provide you with enough information to setup a development environment configured to contribute to SPE. We'll begin with a single installation of Sitecore 8+. Adam wrote an [article](http://blog.najmanowicz.com/2015/03/03/set-up-sitecore-powershell-extensions-development-environment/) \(written March 2015\) that goes into great detail about the installation steps, so we went ahead and added it to the book.

The solution requires Visual Studio 2015 or later.

### Single Instance for Sitecore 8

1. Meet the prerequisites found [here](installation.md)
2. Install Sitecore 8+ to a folder of your choice, for example `C:\inetpub\wwwroot\SitecoreSPE`
3. Clone the repo to your local development environment
   ```
   cd "C:\Source\Console"
   git init
   git remote add origin https://github.com/SitecorePowerShell/Console.git
   git fetch origin
   git checkout -b master --track origin/master
   ```
4. Copy `C:\Source\Console\deploy.user.json.sample` to `C:\Source\Console\deploy.user.json`
5. Edit the sites definition in `deploy.user.json` to target your Sitecore installation folder
6. Compile the solution in Visual Studio
   * Because SPE supports Sitecore 7+, you'll need to unload the `Cognfide.PowerShell.Sitecore7` project to build at this stage. To setup support for Sitecore 7, follow the next section in this tutorial.
   * When you compile the solution, SPE will be automatically deployed to the installation path you set in `deploy.user.json`

   > SPE uses Unicorn for serializing Sitecore items to the source folder, and for syncing items from disk into Sitecore. For more information on Unicorn, see https://github.com/kamsar/Unicorn
7. Login to Sitecore and then navigate to `/Unicorn.aspx`. Use Unicorn to sync all projects into Sitecore.

> SPE uses Unicorn for serializing Sitecore items to the source folder, and for syncing items from disk into Sitecore. For more information on Unicorn, see https://github.com/kamsar/Unicorn

You can now use SPE in your Sitecore installation!

### Multiple Instance

1. Meet the prerequisites found [here](installation.md)
2. Install Sitecore 7+ to the following paths    
   * `C:\inetpub\wwwroot\Sitecore8`
   * `C:\inetpub\wwwroot\Sitecore75`
   * `C:\inetpub\wwwroot\Sitecore70`
3. Fetch source code to installation path
   ```
   cd "C:\Projects\SitecorePowerShell\"
   git init
   git remote add origin https://github.com/SitecorePowerShell/Console.git
   git fetch origin
   git checkout -b master --track origin/master
   ```
4. Copy `C:\Projects\SitecorePowerShell\deploy.targets.sample` to `C:\Projects\SitecorePowerShell\deploy.targets`
5. Edit the paths in `deploy.targets` with the appropriate _SitecorePath_ and _LibrariesPath_
6. Copy Sitecore dependencies to `C:\Projects\SitecorePowerShell\Libraries` with a subfolder for each Sitecore version \(i.e. \SC8, \SC75, \SC7\). Use the reference path in Visual Studio to match the right assembly versions in `Cognifide.PowerShell.Sitecore7` and `Cognifide.PowerShell.Sitecore8`. Not doing so will result in error messages related to those projects.
   * Sitecore.Analytics.dll
   * Sitecore.Client.dll
   * Sitecore.ContentSearch.dll
   * Sitecore.ContentSearch.Linq.dll
   * Sitecore.ExperienceEditor.dll
   * Sitecore.Kernel.dll
   * Sitecore.Logging.dll
   * Sitecore.NVelocity.dll
   * Sitecore.Update.dll
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



