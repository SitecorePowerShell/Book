# Troubleshooting

### General

We generally see issues occurring due to an incompatible version of Windows PowerShell. Be sure to install version 3 or later.

### Installation

#### Hanging Installation Wizard

Some users have reported an [issue](https://github.com/SitecorePowerShell/Console/issues/404) where the package installation in Sitecore hangs while installing SPE. One possible fix is to disable the Sitecore Analytics feature; this of course assumes you do not plan on using it for your instance.

**Article:** Martin Miles encountered the issue and proposed a fix [here][1].

**Hack: ** Run this script on Sitecore 8.0.

```powershell
$paths = @("C:\inetpub\wwwroot\Console\Website\App_Config\Include\*")
$patterns = @("Sitecore.Analytics*.config", "Sitecore.ExperienceAnalytics*.config")
 
$paths | Get-ChildItem -Include $patterns -Recurse | Rename-Item -NewName { $PSItem.Name + ".disabled" }
```

### Exception Messages

#### ISE throws exception when /sitecore/content/home is missing

- [378](https://github.com/SitecorePowerShell/Console/issues/378) - Missing Home item (fixed in 3.2)

### Modules

#### Integration Point is not working

Be sure the module is enabled and the integrations are rebuilt from within the ISE.

[1]: http://blog.martinmiles.net/post/sitecore-8-re-indexing-errors-out-and-module-installation-never-ends-without-mongodb-running