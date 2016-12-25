# ![](images/logos/logo-45x45.jpg) Sitecore PowerShell Extensions: a CLI and scripting tool

![Sitecore PowerShell Extensions](/images/screenshots/cover/readme-console-ise.png)

The [Sitecore PowerShell Extensions][1] (SPE) module is a Sitecore development accelerator which can drastically increase your productivity and curtail the amount of time it takes to deliver a Sitecore solution. 

The module provides a command line (CLI) and scripting environment (ISE) for automating tasks. SPE works with the Sitecore process, capable of making native calls to the Sitecore API and manipulating files. Running commands and writing scripts follow the standard and well-known Windows PowerShell syntax. Windows PowerShell is a common tool used in IT for desktop and server management, so we decided to stick with that as a framework to build upon.

Below are some of the things you get with SPE:
* [Index Viewer](toolbox.md)
* [Scheduled Task Manager](toolbox.md)
* [Report Runner](reports.md)
* [User Session Manager](toolbox.md)
* [Bulk Rename/Remove/Create Tool](working-with-items.md)
* Data Importer
* ~~Professional Chef~~

#### Bundled Tools

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

#### Community Add-ons

The following are Sitecore modules that enhance the SPE experience.
* [SPE Image Importer][16] : [Himadri Chakrabarti][18]
* [Multi-Item Publish][17] : [Richard Seal][19]
* [ActiveCommerce PowerShell Extensions][20] : [Nick Wesselman][14]

#### Endorsements

*"There is nothing you can not do with PowerShell Console, because you're inside the Sitecore application. You can call the Sitecore API"* - [Alistair Deneys][11] - Sitecore Symposium 2012

Recommended by [John West][12] to use as a tool for [maximizing Sitecore developer productivity][10].

*"Get a job done with a one liner!
Just provisioned a new language for the whole content tree with a one-liner. Whaaaat?
Have to include it as a default install for all sandboxes now."* - [Alex Shyba][1]'s comment on Marketplace

*"Thank you for the GitBook. Invaluable Reference."* - [Nick Wesselman][14]'s [tweet][15]

#### Training Material

The following book should provide you with enough information to use and be  productive with SPE. Don't worry, you will be able to use it without having to write any code.

We have a video series available to help walk you through the module [here][13].

We also maintain a comprehensive list of links to [blogs and videos][2] to help you on your journey to SPE awesomeness. Happy coding!

#### Development Team

* [Adam Najmanowicz][3] | [@adamnaj][7]
* [Michael West][4] | [@michaelwest101][8]

#### Help and Support

See the [Troubleshooting](troubleshooting.md) section for some common fixes.

Questions, comments, and feature requests should be submitted on [GitHub][6]. You may also check out the [SPE Slack channel][21] for more interactive support. Not on Slack yet? Contact us on Twitter and we'll send you an invite.

**Disclaimer:** With great power comes great responsibility – this tool can do a lot of good but also bring harm to a lot of content in a very short time – backup your system before running a script that modifies your content and never run any untested scripts in a production environment! We will not be held responsible for any use of it. Test your scripts on your development server first! Test on an exact copy of production before running scripts on production content.

[1]: https://marketplace.sitecore.net/Modules/Sitecore_PowerShell_console.aspx
[2]: http://blog.najmanowicz.com/sitecore-powershell-console/
[3]: http://blog.najmanowicz.com/
[4]: http://michaellwest.blogspot.com/
[5]: http://sitecorejunkie.com/
[6]: https://git.io/spe
[7]: https://twitter.com/adamnaj
[8]: https://twitter.com/MichaelWest101
[9]: https://twitter.com/mike_i_reynolds
[10]: http://www.sitecore.net/learn/blogs/technical-blogs/john-west-sitecore-blog/posts/2015/02/maximize-sitecore-developer-productivity.aspx
[11]: https://twitter.com/adeneys
[12]: https://twitter.com/sitecorejohn
[13]: https://www.youtube.com/playlist?list=PLph7ZchYd_nCypVZSNkudGwPFRqf1na0b
[14]: https://twitter.com/techphoria414
[15]: https://twitter.com/techphoria414/status/632033887632289792
[16]: https://marketplace.sitecore.net/en/Modules/S/SPE_Image_Uploader_Module10.aspx
[17]: http://www.sitecorenutsbolts.net/2015/12/14/Multi-Item-Publish-with-Sitecore-Powershell-Extensions/
[18]: https://twitter.com/himadric
[19]: https://twitter.com/rich_seal
[20]: https://github.com/ActiveCommerce/activecommerce-powershell-extensions
[21]: https://sitecorechat.slack.com/messages/module-spe/