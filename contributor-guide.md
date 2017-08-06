# Contributor Guide

The following guide should provide you with enough information to setup a development environment configured to contribute to SPE. We'll begin with a single installation of Sitecore 8+.

The solution requires Visual Studio 2015 or later.

### Single Instance for Sitecore 8

1. Meet the prerequisites found [here](installation.md)
2. Install Sitecore 8+ to a folder of your choice, for example `C:\inetpub\wwwroot\SitecoreSPE_8`
3. Clone the repo to your local development environment
   ```
   cd "C:\Source\Console"  // Your local source folder, with a new folder for the solution
   git init
   git remote add origin https://github.com/SitecorePowerShell/Console.git
   git fetch origin
   git checkout -b master --track origin/master
   ```
4. Copy `C:\Source\Console\deploy.user.json.sample` to `C:\Source\Console\deploy.user.json`. This will be a file just for your local environment and will be ignored by git.
5. Edit the sites definition in `deploy.user.json` to target your Sitecore installation folder. Make sure the `version` property is `8`. Remove any other sites in the file that do not apply.

> The `deploy.user.json` file supports deploying SPE to multiple Sitecore installations. For now, we are just deploying to a single instance, but later on in the tutorial we will cover multiple instances. 

5. Copy `C:\Source\Console\UserConfiguration\App_Config\Include\z.Cognifide.PowerShell.Development.User.config.sample` to a file of the same name, without the `.sample` suffix. This file can be edited to add any SPE-specific configuration that you want in your sites, but don't wish to commit back into the repo.

> You may notice there is a %%sourceFolder%% value in this configuration file. This is a special string that gets replaced as part of the SPE deployment with your source folder location. You don't need to update this manually.

6. Compile the solution in Visual Studio

   * Because SPE supports Sitecore 7+, you'll need to unload the `Cognfide.PowerShell.Sitecore7` project to build at this stage. Details of how to setup Sitecore 7 support is provided later on in this tutorial.

> Whenever you compile the solution, SPE will be automatically deployed to the site installation paths you have set in `deploy.user.json`

7. Login to Sitecore
8. Navigate to `/Unicorn.aspx`. Use Unicorn to sync all projects into Sitecore.
   
> SPE uses Unicorn for serializing Sitecore items to the source folder, and for syncing items from disk into Sitecore. For more information on Unicorn, see [https://github.com/kamsar/Unicorn](https://github.com/kamsar/Unicorn)

9. You can now use SPE in your Sitecore installation!

### Multiple Instances, and Sitecore 7 support

The SPE deployment process supports multiple sites and multiple versions of Sitecore. The following steps carry on from above to add further support for another Sitecore site, running version 7.x.

1. Complete the steps for a Single Instance.
2. Install Sitecore 7.x to a folder of your choice, for example `C:\inetpub\wwwroot\SitecoreSPE_7`
3. Edit the sites definition in `deploy.user.json` to add your new Sitecore installation folder. Set the `version` property to `7`.
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
6. Follow steps 6 onward from the **Single Instance** guide above to deploy to your Sitecore 7 installation and sync the SPE items into Sitecore.

> SPE can be deployed to as many Sitecore sites as you like. Each time you first deploy to a new installation, make sure you use Unicorn to sync the latest state of items into Sitecore.

### Optional: PowerShell Remoting Module support

To add the SPE PowerShell Remoting Module scripts into your machine's PowerShell Module path, execute the `.\Setup_Module.ps1` script from the source folder. This will add the `\Modules` folder from source into your `PSModulePath` environment variable. Once this is done, you can use `Import-Module SPE` on your development machine to run the remoting scripts.

### Optional: Junction Support

As part of the SPE deployment process, all of the relevant binary, configuration and sitecore module files are copied over from the projects within the solution. This means that any changes to static files such as JS / CSS files require a full build for these to be deployed to the site. As the build triggers an application pool recycle of your site, this can be a little slow for quick changes.

For more rapid development, you can enable junction deployment on your sites. When this is enabled, rather than copying over the static files, [junction points](https://en.wikipedia.org/wiki/NTFS_junction_point) will be setup for various folders so that the folders within the Sitecore installation are _directly linked_ to the source folder. Any changes made in the solution are seen instantly, because the solution and the site are referencing _the exact same files_.

To enable a junction deployment for a site, add `junction` property to the site definition and set it to `true`:

```
{ 
    "sites": [
        {
            "path": "C:\\inetput\\wwwroot\\SitecoreSPE_8",
            "version": 8,
            "junction": true
        }
    ]
}
```

> Note that with junction deployments, a solution build is still required if you want to deploy any code or `.config` changes.

> It is not currently supported for a junction deployment site to be changed back into a non-junction deployment site. If you wish to do this, you should manually delete the following folders from your Sitecore installation before updating the `junction` property back to `false`: `sitecore modules\PowerShell` and `sitecore modules\Shell\PowerShell`