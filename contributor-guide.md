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

7. Login to Sitecore and then navigate to `/Unicorn.aspx`. Use Unicorn to sync all projects into Sitecore.

> SPE uses Unicorn for serializing Sitecore items to the source folder, and for syncing items from disk into Sitecore. For more information on Unicorn, see [https://github.com/kamsar/Unicorn](https://github.com/kamsar/Unicorn)

You can now use SPE in your Sitecore installation!

### Multiple Instances, and Sitecore 7 support

1. Complete the steps for a Single Instance.
2. Install Sitecore 7 to a folder of your choice, for example `C:\inetpub\wwwroot\SitecoreSPE7`
3. Edit the sites definition in `deploy.user.json` to add your new Sitecore installation folder. Set the version property to 7.
4. Copy the following Sitecore dependencies to the `\Libraries\SC7` folder from your Sitecore installation:
   * Sitecore.Analytics.dll
   * Sitecore.Client.dll
   * Sitecore.ContentSearch.dll
   * Sitecore.ContentSearch.Linq.dll
   * Sitecore.ExperienceEditor.dll
   * Sitecore.Kernel.dll
   * Sitecore.Logging.dll
   * Sitecore.NVelocity.dll
   * Sitecore.Update.dll
5. Make sure the `Cognfide.PowerShell.Sitecore7` project is loaded in the Visual Studio solution
6. Follow steps 6 onward from the **Single Instance **guide above to deploy to your Sitecore 7 installation and sync the SPE items into Sitecore.

> SPE can be deployed to as many Sitecore sites as you like. Each time you first deploy to a new installation, make sure you use Unicorn to sync the latest state of items into Sitecore.



