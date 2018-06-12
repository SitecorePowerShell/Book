# Installation

## Prerequisites

* Windows 7+
* Microsoft .NET Framework 4.5.2+ : [Download](https://www.microsoft.com/en-us/download/details.aspx?id=30653)  
* Windows Management Framework 3+ : [Download](https://www.microsoft.com/en-us/download/details.aspx?id=54616)
  * The link provided will get you up to PowerShell 5
  * [Detecting your PowerShell version](https://stackoverflow.com/questions/1825585/determine-installed-powershell-version)
* PowerShell \[Execution Policy\]\[8\] set to `RemoteSigned`

## Download the Module

The SPE module installs like any other for Sitecore.

[Download](https://marketplace.sitecore.net/Modules/Sitecore_PowerShell_console.aspx) the module from the [Sitecore Marketplace](https://marketplace.sitecore.net/) and install through the _Installation Wizard_.

Following the installation you'll find these new items added to the Sitecore menu:

* Sitecore -&gt; [PowerShell Console](../interfaces/console.md)
* Sitecore -&gt; [PowerShell Toolbox](../modules/integration-points/toolbox.md)
* Sitecore -&gt; Development Tools -&gt; [PowerShell ISE](../interfaces/scripting.md)
* Sitecore -&gt; Reporting Tools -&gt; [PowerShell Reports](../modules/integration-points/reports/)

## Compile Your Own Binaries

You may also clone the project from [GitHub](https://git.io/spe) and compile it. This allows you to access the latest functionality without waiting for a new release. See the following [contributor guide](contributor-guide.md) for instructions on how to get up and running.

## Troubleshooting

See the troubleshooting section [here](../troubleshooting.md)

\[8\]: [https://technet.microsoft.com/en-us/library/ee176961.aspx](https://technet.microsoft.com/en-us/library/ee176961.aspx)

