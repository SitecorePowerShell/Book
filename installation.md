# Installation

### Prerequisites

* Windows 7+
* Microsoft .NET Framework 4.5+ : [Download][2]  
* Windows Management Framework 3+ : [Download][3]
 * The link provided will get you up to PowerShell 4
 * [Detecting your PowerShell version][7]
* PowerShell [Execution Policy][8] set to `RemoteSigned`


### Download the Module

The SPE module installs like any other for Sitecore. 

[Download][1] the module from the [Sitecore Marketplace][4] and install through the _Installation Wizard_.

**Note: ** When upgrading from a v2.x you may find it necessary to remove some old Sheer controls. One quick fix is to run the following command in the Console after installation (since the ISE will not work properly).

```powershell
PS master:\> cd c:\
PS C:\> Remove-Item -Path "$($AppPath)sitecore\shell\Applications\PowerShell" -Recurse
```

Following the installation you'll find these new items added to the Sitecore menu:
* Sitecore -> [PowerShell Console](console.md)
* Sitecore -> [PowerShell Toolbox](toolbox.md)
* Sitecore -> Development Tools -> [PowerShell ISE](scripting.md)
* Sitecore -> Reporting Tools -> [PowerShell Reports](reports.md)

### Compile Your Own Binaries

You may also clone the project from [GitHub][5] and compile it. This allows you to access the latest functionality without waiting for a new release. See the following [contributor guide](contributor-guide.md) for instructions on how to get up and running.

### Troubleshooting

See the troubleshooting section [here](troubleshooting.md)

[1]: https://marketplace.sitecore.net/Modules/Sitecore_PowerShell_console.aspx
[2]: http://www.microsoft.com/en-us/download/details.aspx?id=30653 "Link to version 4.5"
[3]: http://www.microsoft.com/en-us/download/details.aspx?id=40855 "Link to version 4"
[4]: https://marketplace.sitecore.net/
[5]: https://git.io/spe
[6]: #
[7]: http://stackoverflow.com/questions/1825585/determine-installed-powershell-version
[8]: https://technet.microsoft.com/en-us/library/ee176961.aspx