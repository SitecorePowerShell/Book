# Releases

## Version 3.1

### New Features

- [337](https://github.com/SitecorePowerShell/Console/issues/337) Create anti-packages
- [368](https://github.com/SitecorePowerShell/Console/issues/368) Content Editor Warning integration
- [364](https://github.com/SitecorePowerShell/Console/issues/364) Page Editor Notification integration
- [341](https://github.com/SitecorePowerShell/Console/issues/341) New-SecuritySource command for package creation

### Enhancements

- [333](https://github.com/SitecorePowerShell/Console/issues/333) `Write-Log` now defaults to the **DEBUG** log
- [363](https://github.com/SitecorePowerShell/Console/issues/363) Script errors are now logged to the **ERROR** log, which will then show line numbers
- [339](https://github.com/SitecorePowerShell/Console/issues/339) `New-FileSource` and `New-ExplicitFileSource` commands support `InstallMode` for package creation
- [338](https://github.com/SitecorePowerShell/Console/issues/338) `Export-Package` command supports `NoClobber` for package creation
- [336](https://github.com/SitecorePowerShell/Console/issues/336) Event settings moved to a separate include file
- [361](https://github.com/SitecorePowerShell/Console/issues/361) `Find-Item` command now supports **Contains** filter
- [334](https://github.com/SitecorePowerShell/Console/issues/334) System Maintenance Module now contains optimization scripts
- [350](https://github.com/SitecorePowerShell/Console/issues/350) New version specific library introduced for compatibility

### Fixes/Changes

- [366](https://github.com/SitecorePowerShell/Console/issues/366) **LoggingIn**, **LoggedIn**, and **LoggedOut** pipelines now use the variable `$pipelineArgs` rather than `$args`
- [365](https://github.com/SitecorePowerShell/Console/issues/365) Prescript functionality removed from **ISE**, **Console**, and **Default** settings
- [357](https://github.com/SitecorePowerShell/Console/issues/357) `Find-Item` command no longer throws *Operation Unsupported* warning
- [358](https://github.com/SitecorePowerShell/Console/issues/358) `Remove-RoleMember` command did not properly remove users from roles


#### Create anti-packages

This is one of those exciting new features, mainly because SPE has had the ability to create packages for a long time, so why not the reverse?! A new library needs to be included in order for the a delete post step to work. It works very much like you find in Sitecore Rocks. If you haven't used that you are missing out on a great tool. 

#### Content/Page Editor Message integration

Now it's possible to write a script for generating warnings in the Content Editor and notifications in the Page Editor. We've included a new module in SPE called License Expiration that utilizes the new functionality into something useful. Every module does include a disable feature.


#### Content Editor Warning


Page Editor Notification


#### Package up security

The package commands now include a new one called New-SecuritySource which adds security settings to packages.

#### Script errors log to the ERROR log

One of the pain points I noticed was that script errors encountered by scripts running outside of the Console and ISE provided insufficient details. Now you can find the error details and with line numbers in the log.

#### New config file and libraries introduced

In order to provider better maintenance or SPE we decided to make a few adjustments. Nothing too crazy.

First we split the functionality that was different in Sitecore versions (like the Rules engine) into a separate library called Cognifide.PowerShell.VersionSpecific.dll. Doing this allowed us to maintain a single branch of code, where the version specific libraries are compiled with their respective Sitecore versions. 

Second we split the Cognifide.PowerShell.config by removing previously unused events section. There are 3 or so events that SPE depends on, so we kept that in the original file. Any of the events you wish to use like "item:saved" will now reside in Cognifide.PowerShell.Events.config.

Third we added a new library called Cognifide.PowerShell.Package.dll which enables the deletion of items and files when installing anti-packages. The library does not have any dependencies other than the Sitecore libraries.

#### Prescript functionality removed

We decided that the Prescript functionality was unlikely to be widely used and only introduced confusion when scripts executed. In some situations your prescript would execute, and in others it would never execute. It's gone.
