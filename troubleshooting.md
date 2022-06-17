# Troubleshooting

## General

We generally see issues occurring due to an incompatible version of Windows PowerShell. Be sure to install Windows PowerShell version 3 or newer.

## Installation

### Elevated Session Hanging

There is an additional configuration file added to support Identity Server. If you are installing Sitecore 9.1 or later you will want to enable the configuration file `Spe.IdentityServer.config`.

{% hint style="info" %}
You can see the Sitecore Stack Exchange answer [here](https://sitecore.stackexchange.com/questions/17984/unable-to-elevate-console-in-9-1-azure-paas/18775#18775) describing the contents of the configuration.
{% endhint %}

### Hanging Installation Wizard

Some users have reported an [issue](https://github.com/SitecorePowerShell/Console/issues/404) where the package installation in Sitecore hangs while installing SPE. One possible fix is to disable the Sitecore Analytics feature; this of course assumes you do not plan on using it for your instance.

**Article:** Martin Miles encountered the issue and proposed a fix [here](https://github.com/SitecorePowerShell/Book/tree/5daee3160885dadd7031fee723dccf12a33abd7b/[https:/blog.martinmiles.net/post/sitecore-8-re-indexing-errors-out-and-module-installation-never-ends-without-mongodb-running]/README.md).

**Hack:**  Run this script on Sitecore 8.0.

```powershell
$paths = @("C:\inetpub\wwwroot\Console\Website\App_Config\Include\*")
$patterns = @("Sitecore.Analytics*.config", "Sitecore.ExperienceAnalytics*.config")

$paths | Get-ChildItem -Include $patterns -Recurse | Rename-Item -NewName { $PSItem.Name + ".disabled" }
```

## Exception Messages

### ISE throws exception when /sitecore/content/home is missing

* [378](https://github.com/SitecorePowerShell/Console/issues/378) - Missing Home item \(fixed in 3.2\)

## Modules

### Integration Point is not working

Be sure the module is enabled and the integrations are rebuilt from within the ISE. The following are some of the integration issues you may experience:

* Content Editor Gutter - Entries not listed.
* Content Editor Ribbon - Buttons not visible or working.
* Control Panel - Entries not listed.
* Functions - Import-Function _name_ parameter not populating with functions or can't be found after running the command.
* Web API - Scripts not existing or can't be found.

### Remoting

* **404** error in browser console can be caused by missing SPE files, a custom pipeline, or perhaps a rewrite rule.
* **"The request failed with an empty response."** could be caused when TLS is offloaded to a load balancer and traffic to the endpoint is no longer over HTTPS. Fixed by issue \#[1005](https://github.com/SitecorePowerShell/Console/issues/1005).
* **"The underlying connection was closed: An unexpected error occurred on a send."** could be caused in Azure PaaS when requests are made using TLS 1.1 or lower. Setting the SecurityProtocol may help. Thanks to Jayesh Sheth for pointing to a resolution.
  * `[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12`



