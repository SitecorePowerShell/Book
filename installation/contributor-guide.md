# Contributor Guide

{% hint style="warning" %}
Support for Sitecore 7 has discontinued with SPE 5.0.
{% endhint %}

The following guide should provide you with enough information to setup a development environment configured to contribute to SPE. We'll begin with a single installation of Sitecore 8+.

The solution requires Visual Studio 2015 or later.

> This guide assumes a source root location of "C:\Source" and a site root location of "C:\inetput\wwwroot". These are not prerequisites of the project, and you can use whatever folder locations suit you.

## Single Instance for Sitecore 8 & 9

1. Meet the prerequisites found [here](./)
2. Install Sitecore 8+ to a folder of your choice, for example `C:\inetpub\wwwroot\SitecoreSPE_8`
3. Clone the repo to your local development environment

   ```powershell
   cd "C:\Source\Console"  // Your local source folder, with a new folder for the solution
   git init
   git remote add origin https://github.com/SitecorePowerShell/Console.git
   git fetch origin
   git checkout -b master --track origin/master
   ```

4. Copy `C:\Source\Console\deploy.user.json.sample` to `C:\Source\Console\deploy.user.json`. This will be a file just for your local environment and will be ignored by git.
5. Edit the sites definition in `deploy.user.json` to target your Sitecore web root folder, making sure you use double-slashes for paths like in the existing file. For this site, make sure the `version` property is `8`. Remove any other sites in the file that do not apply.

   > The `deploy.user.json` file supports deploying SPE to multiple Sitecore installations. For now, we are just deploying to a single instance, but later on in the tutorial we will cover multiple instances.

6. Copy `C:\Source\Console\UserConfiguration\App_Config\Include\z.Spe.Development.User.config.sample` to a file of the same name, without the `.sample` suffix. This file can be edited to add any SPE-specific configuration that you want in your sites, but don't wish to commit back into the repo.

   > You may notice there is a %%sourceFolder%% value in this configuration file. This is a special string that gets replaced as part of the SPE deployment with your source folder location. You don't need to update this manually.

7. Open the solution in Visual Studio.
8. Compile the solution. Whenever you compile the solution, SPE will be automatically deployed to the site web root paths you have set in `deploy.user.json`
9. Login to Sitecore
10. Navigate to `/Unicorn.aspx`. Use Unicorn to sync all projects into Sitecore.

    > SPE uses Unicorn for serializing Sitecore items to the source folder, and for syncing items from disk into Sitecore. For more information on Unicorn, see [https://github.com/kamsar/Unicorn](https://github.com/kamsar/Unicorn)

11. SPE is now installed in Sitecore and you're ready for developing!

Any changes you make going foward just require a build of the solution. Remember that when pulling down updates to the source, you should execute a Unicorn sync to ensure your items are up to date.

## Multiple Instances

The SPE deployment process supports multiple sites and multiple versions of Sitecore. The following steps carry on from above to add further support for another Sitecore site, such as 8.x or 9.x.

1. Complete the steps for a Single Instance.
2. Install Sitecore 8.x/9.x to a folder of your choice, for example `C:\inetpub\wwwroot\SitecoreSPE_91`
3. Edit the sites definition in `deploy.user.json` to add your new Sitecore web root folder. Set the `version` property to `9.0`, `9.1` or `9.2` depending on the major/minor version.
4. Follow steps 7 onward from the **Single Instance** guide above to deploy to your Sitecore 8.x/9.x installation and sync the SPE items into Sitecore.

> SPE can be deployed to as many Sitecore sites as you like. Each time you first deploy to a new installation, make sure you use Unicorn to sync the latest state of items into Sitecore.

## Optional: PowerShell Remoting support

To add the SPE PowerShell Remoting Module scripts into your machine's PowerShell Module path, execute the `.\Setup_Module.ps1` script from the source folder. This will add the `\Modules` folder from source into your `PSModulePath` environment variable. Once this is done, you can use `Import-Module SPE` on your development machine to run the remoting scripts.

## Optional: Junction Support

As part of the SPE deployment process, all of the relevant binary, configuration and sitecore module files are copied over from the projects within the solution. This means that any changes to static files such as JS / CSS files require a full build for these to be deployed to the site. As the build triggers an application pool recycle of your site, this can be a little slow for quick changes.

For more rapid development, you can enable junction deployment on your sites. When this is enabled, rather than copying over the static files, [junction points](https://en.wikipedia.org/wiki/NTFS_junction_point) will be setup for various folders so that the folders within the Sitecore installation are _directly linked_ to the source folder. Any changes made in the solution are seen instantly, because the solution and the site are referencing _the exact same files_.

To enable a junction deployment for a site, add `junction` property to the site definition and set it to `true`:

```json
{ 
    "sites": [
        {
            "path": "C:\\inetput\\wwwroot\\SitecoreSPE_8\\Website",
            "version": 8,
            "junction": true
        }
    ]
}
```

> Note that with junction deployments, a solution build is still required if you want to deploy any code or `.config` changes.
>
> It is not currently supported for a junction deployment site to be changed back into a non-junction deployment site. If you wish to do this, you should manually delete the following folders from your Sitecore installation before updating the `junction` property back to `false`: `sitecore modules\PowerShell` and `sitecore modules\Shell\PowerShell`

