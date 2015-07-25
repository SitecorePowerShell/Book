#### Troubleshooting

##### Hanging Installation
Some users have reported an [issue](https://github.com/SitecorePowerShell/Console/issues/404) where the package installation in Sitecore hangs while installing SPE. One possible fix is to disable the Sitecore Analytics feature; this of course assumes you do not plan on using it for your instance.

```powershell
$paths = @("C:\inetpub\wwwroot\Console\Website\App_Config\Include\*")
$patterns = @("Sitecore.Analytics*.config", "Sitecore.ExperienceAnalytics*.config")
 
$paths | Get-ChildItem -Include $patterns -Recurse | Rename-Item -NewName { $PSItem.Name + ".disabled" }
```

#### ISE throws exception when /sitecore/content/home is missing

- [378](https://github.com/SitecorePowerShell/Console/issues/378) - Missing Home item (fixed in 3.2)