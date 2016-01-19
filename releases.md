# Releases

Thank you for taking the time to check out the latest and greatest changes for SPE. Please provide your feedback and recommend SPE on Twitter and the [Marketplace][1] or report issues on [Github][2]. Past releases are availabile [here](past-releases.md).

## Version 4.0

### Summary of important changes

#### Breaking Changes

- The `Rebuild-SearchIndex` command now returns an existing job [558](https://github.com/SitecorePowerShell/Console/issues/558) for an index if currently rebuilding.
- Request related variables such as `$HttpContext` are no longer added to the session [557](https://github.com/SitecorePowerShell/Console/issues/557) but can still be accessed through their static context properties.
- The `Publish-Item` command now supports the additional publishing options [539](https://github.com/SitecorePowerShell/Console/issues/539) introduced in Sitecore 7.1. The default behavior was changed to synchronous and requires the `AsJob` parameter to run asynchronously.
- The `Add-ItemLanguage` and `Remove-ItemLanguage` commands were renamed [532](https://github.com/SitecorePowerShell/Console/issues/532) to `Add-ItemVersion` and `Remove-ItemVersion` since item versions and languages are managed almost the same.

#### New Features

- A new Experience Button integration point was added [546](https://github.com/SitecorePowerShell/Console/issues/546).
- Method signature in the ISE received a face lift [541](https://github.com/SitecorePowerShell/Console/issues/541). Now you can see the argument data types!
- Type names now autocomplete [540](https://github.com/SitecorePowerShell/Console/issues/540).

#### Enhancements

- We went crazy on the styling of the console windows so now you can [565](https://github.com/SitecorePowerShell/Console/issues/565) configure the foreground and background colors. The `Show-Result` command also retains [566](https://github.com/SitecorePowerShell/Console/issues/566) the colors of the host session.
- Variables are now hidden from the autocomplete [560](https://github.com/SitecorePowerShell/Console/issues/560) unless the token begins with a `$`.
- The `ScriptSession` now contains a property *LastErrors* [559](https://github.com/SitecorePowerShell/Console/issues/559) that contains the errors encountered in a script rather than cluttering the log.
- The `Publish-Item` command suggests database names [553](https://github.com/SitecorePowerShell/Console/issues/553) and *filesystem* is no longer an option.
- The export command in reports no longer require a temporary file [551](https://github.com/SitecorePowerShell/Console/issues/551). This should now add support in hosted environments such as Azure [549](https://github.com/SitecorePowerShell/Console/issues/549).
- The `Show-ListView` command now enumerates collections [544](https://github.com/SitecorePowerShell/Console/issues/544) rather than printing the class name.
- The `UseDefaultCredentials` parameter is now supported [531](https://github.com/SitecorePowerShell/Console/issues/531) when using the `New-ScriptSession` command in SPE Remoting.
- The `Publish-Item` command will not publish to all targets by default [530](https://github.com/SitecorePowerShell/Console/issues/530) without requiring the target to be specified.

#### Fixes

- The SPE Remoting commands `Send-RemoteItem` and `Receive-RemoteItem` now use `Write-Error` when an error is encountered [564](https://github.com/SitecorePowerShell/Console/issues/564).
- The `Show-Result` command was not adhering to the font settings [552](https://github.com/SitecorePowerShell/Console/issues/552).
- The `GetItemWorkflowEvent` command threw exceptions for missing optional parameters [545](https://github.com/SitecorePowerShell/Console/issues/545).
- Automatic properties no longer require a restart to update [534](https://github.com/SitecorePowerShell/Console/issues/534).

## Version 3.3
Yet another little-over-a-month update and in many aspects a major one!

[Full issue list available on GitHub](https://github.com/SitecorePowerShell/Console/issues?q=milestone%3A3.3+is%3Aclosed) - 35 issues and new contributing members [@Kasaku](https://github.com/Kasaku) & [@marrrcin](https://github.com/marrrcin)!

### Summary of important changes

#### New Features
- Major re-work to our script session management engine allowing us to [463](https://github.com/SitecorePowerShell/Console/issues/463) spin up new script sessions from your scripts. This allows you to divide scripts into asynchronous parts that can execute in parallel or run operations that would otherwise [462](https://github.com/SitecorePowerShell/Console/issues/462) cause your remoting operations to time out. 
- You can even [475](https://github.com/SitecorePowerShell/Console/issues/475) run more interactive sessions from an existing interactive session!
- The [454](https://github.com/SitecorePowerShell/Console/issues/454) remoting web service added the ability to download large files streamed from server rather than serialized in a SOAP envelope and this has already been [461](https://github.com/SitecorePowerShell/Console/issues/461) conveniently exposed using our remoting module!
- Our new member has contributed a cool functionality that [472](https://github.com/SitecorePowerShell/Console/issues/472) allows you to reset fields and layouts to standard values.
- More of the PowerShell scripts that you can find online will also work OOTB for you as we have implemented the logic that [471](https://github.com/SitecorePowerShell/Console/issues/471) allows the `Read-Host` command to run in our interactive sessions. This command will now pop-up a dialog asking you to provide a line of text like it would in Windows PowerShell.
- Your scripts using [473](https://github.com/SitecorePowerShell/Console/issues/473) `Read-Variable` can now request for strings from users that will be hidden from the prying eyes of bystanders when you specify the `Editor` to be `password`. The typed characters will be replaced with the standard password masking characters in the editor. Your script will still receive a proper string back though.

#### Enhancements
- One weakness the script session management exposed particularly was that [479](https://github.com/SitecorePowerShell/Console/issues/479) interactive cmdlets like `Read-Variable` would cause a script session to hang in non interactive scenario as there would be no UI to handle the message from the script. This has now been fixed and [475](https://github.com/SitecorePowerShell/Console/issues/475) sessions are aware whether they are interactive or not.
- To compliment the new functionality we have [467](https://github.com/SitecorePowerShell/Console/issues/467) extended the Background Session Manager available from the toolbox to show next to each session whether it is busy running a script or not.
- [476](https://github.com/SitecorePowerShell/Console/issues/476) Script sessions are now also aware whether they are busy running script or not and will gracefully reject another script being sent to them if asked to execute one.
- it is now significantly easier to find the item template when using the `New-Item` command as the `-ItemType` parameter now features a fully dynamic autocompletion.
- The [448](https://github.com/SitecorePowerShell/Console/issues/448) Broken Links report now can analyse all versions, not just the latest one.
- The [447](https://github.com/SitecorePowerShell/Console/issues/447) Lock-Item command now supports transferring a lock to another user. If the item is piped to it that is already locked - using the `-Force` parameter will cause the lock to be transferred to the new user specified in the command.
- Some of the [446](https://github.com/SitecorePowerShell/Console/issues/446) commonly used types have gotten [type accelerators](http://blogs.technet.com/b/heyscriptingguy/archive/2013/07/08/use-powershell-to-find-powershell-type-accelerators.aspx) specified for them so you can use those and cast to them much easier.
- Last but not least our second new contributor @marrrcin has contributed [444](https://github.com/SitecorePowerShell/Console/issues/444) a way to Get-ChildItem by providing item ID with the `-ID` parameter rather than using the path.
#### Fixes
- We discovered that when removing more than one rendering the [453](https://github.com/SitecorePowerShell/Console/pull/453) `Remove-Rendering` command encountered an error .
- The very useful [455](https://github.com/SitecorePowerShell/Console/issues/455) `Get-ItemReference` command returned the wrong item, so we fixed that.
- [Lots and lots more of them... actually 10 more!](https://github.com/SitecorePowerShell/Console/issues?q=milestone%3A3.3+is%3Aclosed+label%3Abug)

#### Potential Breaking Changes
- You may find that the changes to [463](https://github.com/SitecorePowerShell/Console/issues/463) `Get-ScriptSession` syntax can potentially interfere with scripts using it.

[1]: https://marketplace.sitecore.net/en/Modules/Sitecore_PowerShell_console.aspx
[2]: https://git.io/spe