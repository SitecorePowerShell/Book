# Releases

Thank you for taking the time to check out the latest and greatest changes for SPE. Please provide your feedback and recommend SPE on Twitter and the [Marketplace](https://marketplace.sitecore.net/en/Modules/Sitecore_PowerShell_console.aspx) or report issues on [Github](https://github.com/SitecorePowerShell/Book/tree/9c7126d7a38df6ef372e8baef52f9a02baabd550/[https:/git.io/spe]/README.md).

## Version 5.1

This release includes new features and bug fixes.

**Note:** This release has a single package for Sitecore 8.x - 9.x and the VersionSpecific dll is an embedded resource. Only install this version if using Sitecore 8 or 9. We've also updated the package a few times due to late discoveries in the the contents.

- [Full list of issues addressed](https://github.com/SitecorePowerShell/Console/issues?q=is%3Aissue+is%3Aclosed+milestone%3A5.1)
- [Release highlights](https://github.com/SitecorePowerShell/Console/issues?q=is%3Aissue+label%3A-release-highlight+is%3Aclosed+milestone%3A5.1)

### Summary of important changes

#### [Breaking Changes](https://github.com/SitecorePowerShell/Console/issues?q=is%3Aissue+is%3Aclosed+milestone%3A5.1+is%3Aissue%20label%3Aimpact-breaking-change)
- SPE Remoting included some commands that need to go away. Removed them in this release. #1072 

#### [Major new Features](https://github.com/SitecorePowerShell/Console/issues?q=is%3Aissue+label%3A-release-highlight+is%3Aclosed+milestone%3A5.1)
- SPE Remoting no longer uses the SOAP service for Invoke-RemoteScript. #1062 
- SPE Web API caches responses for a short period of time to improve performance. #1081 
- SPE Remoting passes credentials via header rather than query string. #1077 

#### [All improvements](https://github.com/SitecorePowerShell/Console/issues?q=is%3Aissue+label%3Aimprovement+is%3Aclosed+milestone%3A5.1)

#### [All Fixes](https://github.com/SitecorePowerShell/Console/issues?q=is%3Aissue+label%3Abug+is%3Aclosed+milestone%3A5.1)

## Version 5.0

It's been exactly one year since the last release of SPE. In this version we added some powerful new features, along with improvements to the UI \(icons, styles\).

**Note**: This major release dropped support for **Sitecore 7**. Only install this version if using Sitecore 8 or 9.

* [Full issue list available on GitHub](https://github.com/SitecorePowerShell/Console/issues?q=is%3Aissue+is%3Aclosed+milestone%3A5.0)
* [Release highlights](https://github.com/SitecorePowerShell/Console/issues?q=is%3Aissue+label%3A-release-highlight+is%3Aclosed+milestone%3A5.0)

### Summary of important changes

#### [Breaking Changes](https://github.com/SitecorePowerShell/Console/issues?q=is%3Aissue+is%3Aclosed+milestone%3A5.0+is%3Aissue%20label%3Aimpact-breaking-change)

* No breaking changes

#### [Major new Features](https://github.com/SitecorePowerShell/Console/issues?q=is%3Aissue+label%3A-release-highlight+is%3Aclosed+milestone%3A5.0)

#### [All improvements](https://github.com/SitecorePowerShell/Console/issues?q=is%3Aissue+label%3Aimprovement+is%3Aclosed+milestone%3A5.0)

#### [All Fixes](https://github.com/SitecorePowerShell/Console/issues?q=is%3Aissue+label%3Abug+is%3Aclosed+milestone%3A5.0)

## Version 4.7

This release has a major focus on stability and overcoming any problems that were reported by our users.

* [Full issue list available on GitHub](https://github.com/SitecorePowerShell/Console/issues?q=is%3Aissue+is%3Aclosed+milestone%3A4.7)
* [Release highlights](https://github.com/SitecorePowerShell/Console/issues?q=is%3Aissue+label%3A-release-highlight+is%3Aclosed+milestone%3A4.7)

### Summary of important changes

#### [Breaking Changes](https://github.com/SitecorePowerShell/Console/issues?q=is%3Aissue+is%3Aclosed+milestone%3A4.7+is%3Aissue%20label%3Aimpact-breaking-change)

#### [Major new Features](https://github.com/SitecorePowerShell/Console/issues?q=is%3Aissue+label%3A-release-highlight+is%3Aclosed+milestone%3A4.7)

* The Packing Manager received a nice facelift as part of issue [923](https://github.com/SitecorePowerShell/Console/issues/923). Better icons and more options.

#### [All improvements](https://github.com/SitecorePowerShell/Console/issues?q=is%3Aissue+label%3Aimprovement+is%3Aclosed+milestone%3A4.7)

#### [All Fixes](https://github.com/SitecorePowerShell/Console/issues?q=is%3Aissue+label%3Abug+is%3Aclosed+milestone%3A4.7)
