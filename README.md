# Introduction

![Sitecore PowerShell Extensions](.gitbook/assets/readme-console-ise.png)

The [Sitecore PowerShell Extensions](https://marketplace.sitecore.net/Modules/Sitecore_PowerShell_console.aspx) \(SPE\) module is a Sitecore development accelerator which can drastically increase your productivity and curtail the amount of time it takes to deliver a Sitecore solution.

The module provides a command line \(CLI\) and scripting environment \(ISE\) for automating tasks. SPE works with the Sitecore process, capable of making native calls to the Sitecore API and manipulating files. Running commands and writing scripts follow the standard and well-known Windows PowerShell syntax. Windows PowerShell is a common tool used in IT for desktop and server management, so we decided to stick with that as a framework to build upon.

Below are some of the things you get with SPE:

* [Index Viewer](modules/integration-points/toolbox.md)
* [Scheduled Task Manager](modules/integration-points/toolbox.md)
* [Report Runner](modules/integration-points/reports/)
* [User Session Manager](modules/integration-points/toolbox.md)
* [Bulk Rename/Remove/Create Tool](working-with-items/)
* Data Importer
* ~~Professional Chef~~

## Bundled Tools

The following are some helpful modules distributed with SPE.

* **Bundle**
  * Authoring Instrumentation
  * Copy Renderings
  * Package Generator
  * Publishing Status Gutter
* **Core**
  * Platform
* **Maintenance**
  * Index On Demand
  * Media Library Maintenance
  * System Maintenance
  * Task Management
  * User Management
  * User Session Management
* **Reporting**
  * Authorable Reports - reports based on the Sitecore Rules Engine.
  * Content Reports - variety of reports to help audit the Sitecore solution.
* **Samples**
  * Automatically show quick info section
  * Enforce user password expiration
  * Example Event Handlers
  * Getting Started
  * License Expiration
  * Random desktop background
  * Unlock user items on logout

## Community Add-ons

The following are Sitecore modules that enhance the SPE experience.

* [SPE Image Importer](https://marketplace.sitecore.net/en/Modules/S/SPE_Image_Uploader_Module10.aspx) : [Himadri Chakrabarti](https://twitter.com/himadric)
* [Multi-Item Publish](http://www.sitecorenutsbolts.net/2015/12/14/Multi-Item-Publish-with-Sitecore-Powershell-Extensions/) : [Richard Seal](https://twitter.com/rich_seal)
* [ActiveCommerce PowerShell Extensions](https://github.com/ActiveCommerce/activecommerce-powershell-extensions) : [Nick Wesselman](https://twitter.com/techphoria414)

## Endorsements

_"There is nothing you can not do with PowerShell Console, because you're inside the Sitecore application. You can call the Sitecore API"_ - [Alistair Deneys](https://twitter.com/adeneys) - Sitecore Symposium 2012

Recommended by [John West](https://twitter.com/sitecorejohn) to use as a tool for [maximizing Sitecore developer productivity](http://www.sitecore.net/learn/blogs/technical-blogs/john-west-sitecore-blog/posts/2015/02/maximize-sitecore-developer-productivity.aspx).

_"Get a job done with a one liner!  
Just provisioned a new language for the whole content tree with a one-liner. Whaaaat?  
Have to include it as a default install for all sandboxes now."_ - [Alex Shyba](https://marketplace.sitecore.net/Modules/Sitecore_PowerShell_console.aspx)'s comment on Marketplace

_"Thank you for the GitBook. Invaluable Reference."_ - [Nick Wesselman](https://twitter.com/techphoria414)'s [tweet](https://twitter.com/techphoria414/status/632033887632289792)

## Training Material

The following book should provide you with enough information to use and be productive with SPE. Don't worry, you will be able to use it without having to write any code.

We have a video series available to help walk you through the module [here](https://www.youtube.com/playlist?list=PLph7ZchYd_nCypVZSNkudGwPFRqf1na0b).

We also maintain a comprehensive list of links to [blogs and videos](http://blog.najmanowicz.com/sitecore-powershell-console/) to help you on your journey to SPE awesomeness. Happy coding!

## Development Team

* [Adam Najmanowicz](http://blog.najmanowicz.com/) \| [@adamnaj](https://twitter.com/adamnaj)
* [Michael West](http://michaellwest.blogspot.com/) \| [@michaelwest101](https://twitter.com/MichaelWest101)

## Help and Support

See the [Troubleshooting](troubleshooting.md) section for some common fixes.

Questions, comments, and feature requests should be submitted on [GitHub](https://git.io/spe). You may also check out the \[SPE Slack channel\]\[21\] for more interactive support. Not on Slack yet? Contact us on Twitter and we'll send you an invite.

**Disclaimer:** With great power comes great responsibility – this tool can do a lot of good but also bring harm to a lot of content in a very short time – backup your system before running a script that modifies your content and never run any untested scripts in a production environment! We will not be held responsible for any use of it. Test your scripts on your development server first! Test on an exact copy of production before running scripts on production content.

\[21\]: [https://sitecorechat.slack.com/messages/module-spe/](https://sitecorechat.slack.com/messages/module-spe/)

