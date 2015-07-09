# Installation

#### Prerequisites
* Windows 7+
* Microsoft .NET Framework 4+ : [Download][2]  
* Windows Management Framework 3+ : [Download][3]
 * The link provided will get you up to PowerShell 4
 * [Detecting your PowerShell version][7]
* PowerShell [Execution Policy][8] set to `RemoteSigned`


#### Download the Module
The SPE module installs like any other for Sitecore. 

[Download][1] the module from the [Sitecore Marketplace][4] and install through the _Installation Wizard_.

Following the installation you'll find these new items added to the Sitecore menu:
* Sitecore -> PowerShell Console
* Sitecore -> PowerShell Toolbox
* Sitecore -> Development Tools -> PowerShell ISE
* Sitecore -> Reporting Tools -> PowerShell Reports

#### Compile Your Own Binaries

You may also clone the project from [GitHub][5] and compile it. This allows you to access the latest functionality without waiting for a new release. See the following [article][6] for instructions on how to get up and running.

> $ git clone https://github.com/SitecorePowerShell/Console.git  

#### Troubleshooting

##### Hanging Installation
Some users have reported an [issue](https://github.com/SitecorePowerShell/Console/issues/404) where the package installation in Sitecore hangs while installing SPE. One possible fix is to disable the Sitecore Analytics feature; this of course assumes you do not plan on using it for your instance.

```powershell
$paths = @("C:\inetpub\wwwroot\Console\Website\App_Config\Include\*")
$patterns = @("Sitecore.Analytics*.config", "Sitecore.ExperienceAnalytics*.config")
 
$paths | Get-ChildItem -Include $patterns -Recurse | Rename-Item -NewName { $PSItem.Name + ".disabled" }
```

[1]: https://marketplace.sitecore.net/Modules/Sitecore_PowerShell_console.aspx
[2]: http://www.microsoft.com/en-us/download/details.aspx?id=30653 "Link to version 4.5"
[3]: http://www.microsoft.com/en-us/download/details.aspx?id=40855 "Link to version 4"
[4]: https://marketplace.sitecore.net/
[5]: https://github.com/SitecorePowerShell/Console
[6]: http://blog.najmanowicz.com/2015/03/03/set-up-sitecore-powershell-extensions-development-environment/
[7]: http://stackoverflow.com/questions/1825585/determine-installed-powershell-version
[8]: https://technet.microsoft.com/en-us/library/ee176961.aspx