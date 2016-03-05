# Releases

Thank you for taking the time to check out the latest and greatest changes for SPE. Please provide your feedback and recommend SPE on Twitter and the [Marketplace][1] or report issues on [Github][2]. Past releases are availabile [here](past-releases.md).

## Version 4.0

A lot of hard work went into this release. Wasn't released as soon as we hoped, but we are certain many of you will be pleased with the results. Happy coding!

[Full issue list available on GitHub](https://github.com/SitecorePowerShell/Console/issues?q=is%3Aissue+is%3Aclosed+milestone%3A4.0) - 80+ issues.

### Summary of important changes

#### Breaking Changes

- The `Rebuild-SearchIndex` command now returns an existing job [558](https://github.com/SitecorePowerShell/Console/issues/558) for an index if currently rebuilding.
- Request related variables such as `$HttpContext` are no longer added to the session [557](https://github.com/SitecorePowerShell/Console/issues/557) but can still be accessed through their static context properties.
- The `Publish-Item` command now supports the additional publishing options [539](https://github.com/SitecorePowerShell/Console/issues/539) introduced in Sitecore 7.1. The default behavior was changed to synchronous and requires the `AsJob` parameter to run asynchronously.
- The `Add-ItemLanguage` and `Remove-ItemLanguage` commands were renamed [532](https://github.com/SitecorePowerShell/Console/issues/532) to `Add-ItemVersion` and `Remove-ItemVersion` since item versions and languages are managed almost the same.
- The user management commands received an adjustment [520](https://github.com/SitecorePowerShell/Console/issues/520) [498](https://github.com/SitecorePowerShell/Console/issues/498) [497](https://github.com/SitecorePowerShell/Console/issues/497) in which `New-Role` no longer requires `-Passthru` to output the resulting account. Also, the `Remove-User` command now accepts the `User` object as input.
- User reported [511](https://github.com/SitecorePowerShell/Console/issues/511) that SPE conflicted with the **InSite Commerce** product due to the 2.x services location so we went ahead and broke compatibility.
- Some users have encountered items with multiple fields of the same name [575](https://github.com/SitecorePowerShell/Console/issues/575) so we changed the behavior by prefixing the name with and underscore recurrently to disambiguate them.

#### New Features

- A new Experience Button integration point was added [546](https://github.com/SitecorePowerShell/Console/issues/546).
- Method signature in the ISE received a face lift [541](https://github.com/SitecorePowerShell/Console/issues/541). Now you can see the argument data types!
- Type names now autocomplete [540](https://github.com/SitecorePowerShell/Console/issues/540).
- SPE Remoting now supports uploading large files [528](https://github.com/SitecorePowerShell/Console/issues/528).
- Rebuilding the index for single items is now possible [523](https://github.com/SitecorePowerShell/Console/issues/523) with `Rebuild-SearchIndexItem` and removing with `Remove-SearchIndexItem`.
- Added a report [513](https://github.com/SitecorePowerShell/Console/issues/513) for unpublished items in one or more targets.
- Users can now peek the session variable values by way of a tooltip [495](https://github.com/SitecorePowerShell/Console/issues/495).
- The ISE now shows when a script is modified or not yet saved [458](https://github.com/SitecorePowerShell/Console/issues/458).

#### Enhancements

- We went crazy on the styling of the console windows so now you can [565](https://github.com/SitecorePowerShell/Console/issues/565) configure the foreground and background colors. The `Show-Result` command also retains [566](https://github.com/SitecorePowerShell/Console/issues/566) the colors of the host session.
- Variables are now hidden from the autocomplete [560](https://github.com/SitecorePowerShell/Console/issues/560) unless the token begins with a `$`.
- The `ScriptSession` now contains a property *LastErrors* [559](https://github.com/SitecorePowerShell/Console/issues/559) that contains the errors encountered in a script rather than cluttering the log.
- The `Publish-Item` command suggests database names [553](https://github.com/SitecorePowerShell/Console/issues/553) and *filesystem* is no longer an option.
- The export command in reports no longer require a temporary file [551](https://github.com/SitecorePowerShell/Console/issues/551). This should now add support in hosted environments such as Azure [549](https://github.com/SitecorePowerShell/Console/issues/549)[529](https://github.com/SitecorePowerShell/Console/issues/529).
- The `Show-ListView` command now enumerates collections [544](https://github.com/SitecorePowerShell/Console/issues/544) rather than printing the class name.
- The `UseDefaultCredentials` parameter is now supported [531](https://github.com/SitecorePowerShell/Console/issues/531) when using the `New-ScriptSession` command in SPE Remoting.
- The `Publish-Item` command will not publish to all targets by default [530](https://github.com/SitecorePowerShell/Console/issues/530) without requiring the target to be specified.
- Some of the UI elements using `Show-ListView` can be hidden when using the *Hide* parameter [521](https://github.com/SitecorePowerShell/Console/issues/521).
- Your favorite commands `Get-Item` and `Get-ChildItem` now have [519](https://github.com/SitecorePowerShell/Console/issues/519) tab completion on the `-Language` and `-Database` parameters.
- SPE Remoting supports `$using:[variablename]` in the scriptblock [499](https://github.com/SitecorePowerShell/Console/issues/499).
- Changed the behavior of `Add-ItemLanguage` so that it outputs the existing latest version if it exists [578](https://github.com/SitecorePowerShell/Console/issues/578).
- Sometimes you need to grant Acls for System Roles like *Everyone* and *Creator-Owner* so we've updated  [596](https://github.com/SitecorePowerShell/Console/issues/596) [595](https://github.com/SitecorePowerShell/Console/issues/595) our security commands like `Get-Role` and `New-ItemAcl` to support that scenario.

#### Fixes

- The SPE Remoting commands `Send-RemoteItem` and `Receive-RemoteItem` now use `Write-Error` when an error is encountered [564](https://github.com/SitecorePowerShell/Console/issues/564).
- The `Show-Result` command was not adhering to the font settings [552](https://github.com/SitecorePowerShell/Console/issues/552).
- The `GetItemWorkflowEvent` command threw exceptions for missing optional parameters [545](https://github.com/SitecorePowerShell/Console/issues/545).
- Automatic properties no longer require a restart to update [534](https://github.com/SitecorePowerShell/Console/issues/534).
- Creating new branch items now respects [577](https://github.com/SitecorePowerShell/Console/issues/577) the specified language.
- Adam was going crazy trying to resolve an issue [579](https://github.com/SitecorePowerShell/Console/issues/579) where `Set-Layout` wasn't working. We think he's better now and the doctor released him from the clinic.


[1]: https://marketplace.sitecore.net/en/Modules/Sitecore_PowerShell_console.aspx
[2]: https://git.io/spe