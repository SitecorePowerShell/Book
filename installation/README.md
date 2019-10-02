---
description: Review prerequisites and details on how to get setup with SPE.
---

# Installation

## Prerequisites

* Windows 7+
* Microsoft .NET Framework 4.5.2+ : [Download](https://www.microsoft.com/en-us/download/details.aspx?id=30653)  
* Windows Management Framework 3+ : [Download](https://www.microsoft.com/en-us/download/details.aspx?id=54616)
  * The link provided will get you up to PowerShell 5
  * [Detecting your PowerShell version](https://stackoverflow.com/questions/1825585/determine-installed-powershell-version)
* PowerShell [Execution Policy](https://technet.microsoft.com/en-us/library/ee176961.aspx) set to `RemoteSigned` \(probably optional\)

{% hint style="info" %}
The release of SPE 6.0 introduced name changes to some files which are now reflected throughout the documentation. Review [issue #1109](https://github.com/SitecorePowerShell/Console/issues/1109) to see the full scope of what changed. Any place where the name _Cognifide_ or _Cognifide.PowerShell_ was used is now replaced with _Spe_.
{% endhint %}

{% hint style="warning" %}
Sitecore versions 7.x and below are no longer supported with the release of SPE 5.0.
{% endhint %}

## Installation Walkthrough

We have a short [video tutorial here](https://youtu.be/bVqa4DAANYk) walking through the installation here. The irony is that it's recorded for Sitecore version 7.x which is no longer supported.  ¯\\_\(ツ\)\_/¯ 

## Download the Module

The SPE module installs like any other for Sitecore.

[Download](https://marketplace.sitecore.net/Modules/Sitecore_PowerShell_console.aspx) the module from the [Sitecore Marketplace](https://marketplace.sitecore.net/) and install through the _Installation Wizard_.

Following the installation you'll find these new items added to the Sitecore menu:

* Sitecore &gt; [PowerShell Console](../interfaces/console.md)
* Sitecore &gt; [PowerShell Toolbox](../modules/integration-points/toolbox.md)
* Sitecore &gt; Development Tools &gt; [PowerShell ISE](../interfaces/scripting.md)
* Sitecore &gt; Reporting Tools &gt; [PowerShell Reports](../modules/integration-points/reports/)

## Compile Your Own Binaries

You may also clone the project from [GitHub](https://git.io/spe) and compile it. This allows you to access the latest functionality without waiting for a new release. See the following [contributor guide](contributor-guide.md) for instructions on how to get up and running.

## Troubleshooting

See the troubleshooting section [here](../troubleshooting.md)

