# Releases

Thank you for taking the time to check out the latest and greatest changes for SPE. Please provide your feedback on Twitter and the [Marketplace][1] or report issues on [Github][2].

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
- [463](https://github.com/SitecorePowerShell/Console/issues/463) The changes to `Get-ScriptSession` syntax can potentially interfere with scripts using it



## Version 3.2
We've managed to turnaround a release in a little over a month!

[Full issue list available on GitHub](https://github.com/SitecorePowerShell/Console/issues?q=milestone%3A3.2+is%3Aclosed) - 62 issues closed!

### Summary of important changes

#### New Features

- [383](https://github.com/SitecorePowerShell/Console/issues/383) ISE plugin tab and integration point added - you can now write scripts to process your scripts!
- [390](https://github.com/SitecorePowerShell/Console/issues/390) ISE plugin added to find scripts containing selected text
- [388](https://github.com/SitecorePowerShell/Console/issues/388) ISE plugin added to prettify edited script
- [422](https://github.com/SitecorePowerShell/Console/issues/422) `Expand-Archive` command added
- [411](https://github.com/SitecorePowerShell/Console/issues/411) ISE dialog to search for scripts by name
- [396](https://github.com/SitecorePowerShell/Console/issues/396) `New-Role`, `Test-Role`, `Remove-Role` commands added
- [435](https://github.com/SitecorePowerShell/Console/issues/435) `Get-SearchIndex`, `Rebuild-SearchIndex`, `Stop-SearchIndex`, `Resume-SearchIndex`, `Suspend-SearchIndex` commands added
- [434](https://github.com/SitecorePowerShell/Console/issues/434) `Render-ReportField` function-script added to improve Authorable Reports

#### Enhancements

- [438](https://github.com/SitecorePowerShell/Console/issues/438) Console and [436](https://github.com/SitecorePowerShell/Console/issues/436) ISE results are output in real time
- [421](https://github.com/SitecorePowerShell/Console/issues/421) `Read-Variable` dialogs can now show variable editors in a grid-like manner where you can assign 1-12 columns per variable editor
- [413](https://github.com/SitecorePowerShell/Console/issues/413) `Read-Variable` dialogs showing  non-editable messages (for additional dialog explanatory messages where needed)
- [259](https://github.com/SitecorePowerShell/Console/issues/259) `Read-Variable` dialogs can now have mandatory fields
- [427](https://github.com/SitecorePowerShell/Console/issues/427) `Read-Variable` dialogs can now have placeholder text.
- [412](https://github.com/SitecorePowerShell/Console/issues/412) `Show-ListView` can now contain links executing actions in a specific report row
- [382](https://github.com/SitecorePowerShell/Console/issues/382) SPE Remoting module improvements
- [415](https://github.com/SitecorePowerShell/Console/issues/415) `Find-Item` criteria now supports inverted *Filter*
- [418](https://github.com/SitecorePowerShell/Console/issues/418) Improved formatting for `Find-Item` results
- [379](https://github.com/SitecorePowerShell/Console/issues/379) SPE Remoting support for Windows Authentication
- [417](https://github.com/SitecorePowerShell/Console/issues/417) Verbose messages are now yellow and black
- [391](https://github.com/SitecorePowerShell/Console/issues/391) `Find-Item` *DecendentOf* criteria added for faster searches
- [424](https://github.com/SitecorePowerShell/Console/issues/424) `Find-Item` supports Fuzzy and Date search
- [401](https://github.com/SitecorePowerShell/Console/issues/401) `Read-Variable` supports "item" editor that has no value selected on input or output.

#### Fixes

- [420](https://github.com/SitecorePowerShell/Console/issues/420) `Find-Item` *Contains* criteria complained about case sensitivity
- [419](https://github.com/SitecorePowerShell/Console/issues/419) ISE did not format data in the results pane
- [414](https://github.com/SitecorePowerShell/Console/issues/414) ISE buttons would enable at the wrong time
- ... many, many more
- 
#### Potential Breaking Changes

- [376](https://github.com/SitecorePowerShell/Console/issues/376) `Compress-Archive` api change
- [356](https://github.com/SitecorePowerShell/Console/issues/356) Changes in existing commandlets for old Sitecore search APIs - `Get-SearchIndex` repurposed for new api, `Get-Index` and `Get-ItemByUri` are no longer available.

## Version 3.1
It's already June and been way too long since we last released SPE. The SPE team has been hard at work preparing for version 3.1 and we hope this version introduces many welcomed changes long overdue. The most important changes are explained below. As always - please provide any and all feedback either on twitter or GitHub. 

[Full issue list available on GitHub](http://bit.ly/ScPs31Iss)

### Short summary of important changes

#### New Features

- [337](https://github.com/SitecorePowerShell/Console/issues/337) Create anti-packages
- [293](https://github.com/SitecorePowerShell/Console/issues/293) Rules-based report creator
- [318](https://github.com/SitecorePowerShell/Console/issues/318) **Content Editor** "Insert" menu integration 
- [368](https://github.com/SitecorePowerShell/Console/issues/368) **Content Editor** warning integration
- [364](https://github.com/SitecorePowerShell/Console/issues/364) **Experience Editor** notification integration
- [227](https://github.com/SitecorePowerShell/Console/issues/227) New cmdlets for managing Item security allowing for changing which users/roles can do what to which items.
- [341](https://github.com/SitecorePowerShell/Console/issues/341) [`New-SecuritySource`](appendix/commands/New-SecuritySource.html) command added to enable inclusion of Users and Roles in packages
- [324](https://github.com/SitecorePowerShell/Console/issues/324) New SPE Remoting module for use outside of Sitecore
- [372](https://github.com/SitecorePowerShell/Console/issues/372) Most Recently Used scripts are now user specific and be stored in master database with user settings.
- [371](https://github.com/SitecorePowerShell/Console/issues/371) Ability to open and save in ISE scripts stored in different database than the current ContentDatabase.

#### Enhancements

- [326](https://github.com/SitecorePowerShell/Console/issues/326) [`Read-Variable`](appendix/commands/Read-Variable.html) cmdlet provides the ability to edit Sitecore rules.
- [333](https://github.com/SitecorePowerShell/Console/issues/333) [`Write-Log`](appendix/commands/Write-Log.html) now defaults to the **DEBUG** log
- [363](https://github.com/SitecorePowerShell/Console/issues/363) Script errors are now logged to the **ERROR** log, which will then show line numbers
- [339](https://github.com/SitecorePowerShell/Console/issues/339) [`New-FileSource`](appendix/commands/New-FileSource.html) and [`New-ExplicitFileSource`](appendix/commands/New-ExplicitFileSource.html) commands support `InstallMode` for package creation
- [338](https://github.com/SitecorePowerShell/Console/issues/338) [`Export-Package`](appendix/commands/Export-Package.html) command supports `NoClobber` for package creation
- [336](https://github.com/SitecorePowerShell/Console/issues/336) Event settings moved to a separate include file
- [334](https://github.com/SitecorePowerShell/Console/issues/334) Added System Maintenance Module containing instance optimization scripts
- [350](https://github.com/SitecorePowerShell/Console/issues/350) New version specific dll introduced for compatibility
- [316](https://github.com/SitecorePowerShell/Console/issues/316) Ability to set height of a field in dialog created by [`Read-Variable`](appendix/commands/Read-Variable.html)
- [374](https://github.com/SitecorePowerShell/Console/issues/374) [`Show-ModalDialog`](appendix/commands/Show-ModalDialog.html) can pass parameters through Url handles opening more OOTB Sitecore dialogs for re-use.

#### Fixes
- [357](https://github.com/SitecorePowerShell/Console/issues/357) [`Find-Item`](appendix/commands/Find-Item.html) command no longer throws *Operation Unsupported* warning
- [358](https://github.com/SitecorePowerShell/Console/issues/358) [`Remove-RoleMember`](appendix/commands/Remove-RoleMember.html) command did not properly remove users from roles.
- [361](https://github.com/SitecorePowerShell/Console/issues/361) [`Find-Item`](appendix/commands/Find-Item.html) command **Contains** filter indicates properly that case sensitiveness is not Supported by Sitecore if that functionality is used.

#### Potential Breaking Changes
- [365](https://github.com/SitecorePowerShell/Console/issues/365) Prescript functionality removed from **ISE**, **Console**, and **Default** settings
- [365](https://github.com/SitecorePowerShell/Console/issues/373) `core` database no longer can contain scripts
- [366](https://github.com/SitecorePowerShell/Console/issues/366)  LoggingIn, LoggedIn, and LoggedOut pipelines now use the variable $pipelineArgs rather than $args

### Stuff we're so excited about that we needed to tell you more so you don't miss out!

#### Improved documentation

We have gone through most of the documentation wehave created for out cmdlets; fixed and improved those. Just like  you could before - you can put the text cursor after any cmdlet and press Ctrl+Enter to see the help for it - it's just that now you will see a lot more info there with better explanation and examples. We have also consolidated our documentation effort on this book. We will still blog and record videos but we recognize the need for a centralized and are going to act upon it.

#### Create anti-packages

This is one of those exciting new features, mainly because SPE has had the ability to create packages for a long time, so why not the reverse!? A new library needs to be included in order for the a delete post step to work. It works very much like the feature you can find in Sitecore Rocks. If you haven't used that you are missing out on a great tool. 

We started off by adding a new toolbox item.

![Toolbox](images/screenshots/toolbox-list.png)

We enhanced the experience by adding a modal dialog.

![Package Browser](images/screenshots/modaldialog-packages.png)

#### Rules-based report creator

We had this idea to allow users to generate reports without having to know PowerShell. With this simple toolbox item you can generate a new report in under a minute.

After you launch the toolbox item *Rules based report* you are prompted with a dialog to choose the root node along with the rules.

![Rules Based Report](images/screenshots/toolbox-rulesbasedreport.png)

Choose which custom fields and standard fields you wish to be included in the final report.

![Rules Report Fields](images/screenshots/toolbox-rulesreportfields.png)

#### Content Editor Insert Menu Integration
If you put a script in the `Content Editor/Insert Item/` library in an active module it will automatically get exposed to your users when they select an "Insert" option on an item in content tree in **Content Editor**. Just like with other integrations you can control if and when the script appears by setting appropriate rules on the script item.

![Content Editor Insert Menu](images/screenshots/contenteditor-insertmenu.png)

#### Content/Experience Editor Message integration

Now it's possible to write a script for generating warnings in the Content Editor and notifications in the Page Editor. We've included a new module in SPE called License Expiration that utilizes the new functionality into something useful. Every module does include a disable feature.

![Content Editor Notification](images/screenshots/contenteditor-expirewarning.png)

![Experience Editor Notification](images/screenshots/experienceeditor-expirenotification.png)

#### Manage Item Security
The module commands now include a number of comands that allow for managing item security
- [`New-ItemAcl`](appendix/commands/New-ItemAcl.html) - Creates a new access rule for use with [`Set-ItemAcl`](appendix/commands/Set-ItemAcl.html) and [`Add-ItemAcl`](appendix/commands/Add-ItemAcl.html) cmdlets.
- [`Get-ItemAcl`](appendix/commands/Get-ItemAcl.html) - Retrieves security access rules from an item. Those rules can later be used with [`Set-ItemAcl`](appendix/commands/Set-ItemAcl.html) and [`Add-ItemAcl`](appendix/commands/Add-ItemAcl.html) cmdlets to copy them to another item or manipulated and saved back to the same item.
- [`Add-ItemAcl`](appendix/commands/Add-ItemAcl.html) - Adds new access rule to an item allowing for the item to have the access granted or denied for a provided role or user. The new rules will be appended to the existing security descriptors on the item.
- [`Set-ItemAcl`](appendix/commands/Set-ItemAcl.html) - Sets new security information on an item. The new rules will overwrite the existing security descriptors on the item.
- [`Clear-ItemAcl`](appendix/commands/Clear-ItemAcl.html) - Removes all security information from the item specified.
- [`Test-ItemAcl`](appendix/commands/Test-ItemAcl.html) allows script to determine whether the specified user can perform a specified operation.

#### Packaging of Users and Roles
The module commands now include a new one called [`New-SecuritySource`](appendix/commands/New-SecuritySource.html) which adds the ability to store Users and Roles in packages created with SPE.

#### ISE Script opening enhancements
Your Most Recently Used scripts are now specific to your account. This means that if your colleagues are using ISE as well you will no longer see their most recently used scripts.

![Most Recently Used Scripts Gallery](images/screenshots/IseMru1.png)

While we were at it, we've also added the tree view for faster access to your scripts. In the second tab in that gallery you can open scripts really quickly. Also if you are already editing a script - your currently edited script will be highlighted so you can switch between scripts in a single module much quicker. 

![Switch between scripts quicker](images/screenshots/IseMru2.png)

Not stopping there, we have also added the ability to open scripts from other databases. You can see the database selector drop-down in the above screenshot that gives you ability to open scripts in a different database than your current context database. This gives you ability to edit your scripts in the `master` database while you're working in **Content Editor** in `core` database.

The same database switcher has also been added to the full **Open Script** dialog.

#### SPE module for use outside of Sitecore

We have finally introduced a Windows PowerShell module that can be used outside of Sitecore. The module includes commands to interact with the SPE web services and should be the preferred method.

Either from the downloads section on the Sitecore Marketplace or in Github repo you'll find the package `SPE Remoting-3.1.zip`. 

You'll want to extract the contents to your Windows PowerShell module directory as defined in `$env:PSModulePath` or perhaps even add a new path if necessary.

Since I am using the module under my account I can save it my `$home` path:

`C:\Users\Michael\Documents\WindowsPowerShell\Modules\SPE`

Running [`Get-Command`](appendix/commands/Get-Command.html) you can get the list of supported commands:

```ps
PS C:\Windows\system32> Get-Command -Module SPE

CommandType Name                ModuleName
----------- ----                ----------
Function    ConvertFrom-CliXml  SPE
Function    ConvertTo-CliXml    SPE
Function    Invoke-RemoteScript SPE
Function    New-ScriptSession   SPE
Function    Receive-MediaItem   SPE
Function    Send-MediaItem      SPE
```

#### Script errors log to the Sitecore ERROR log

One of the pain points I noticed was that script errors encountered by scripts running outside of the Console and ISE provided insufficient details. Now you can find the error details and with line numbers in the log.

```
4620 19:05:24 ERROR The property 'Icon1' cannot be found on this object. Verify that the property exists and can be set.At line:9 char:1
+ $warning.Icon1 = $icon
+ ~~~~~~~~~~~~~~~~~~~~~
```

#### New config file and libraries introduced

In order to improve how we maintain SPE and to be able to work off of a common branch to support both Sitecore 7 and 8  we've shifted a few things around. Nothing too crazy.

First we split the functionality that was different between major Sitecore versions (like classes moving between assembiles) into a separate dll called `Cognifide.PowerShell.VersionSpecific.dll`. Doing this allowed us to maintain a single branch of code, where the version specific libraries are compiled with their respective Sitecore versions.

Second we've split the `Cognifide.PowerShell.config` by removing previously unused events section. There are 3 or so events that SPE depends on, so we kept that in the original file. Any of the events you wish to use like `"item:saved"` will now reside in `Cognifide.PowerShell.Events.config.disabled`, if you want to enable hooking into Sitecore events simply rename it to `Cognifide.PowerShell.Events.config`.

Third we added a new library called `Cognifide.PowerShell.Package.dll` which enables the deletion of items and files when installing anti-packages. The library does not have any dependencies other than the Sitecore libraries.

### Removed functionality

#### Prescript

We decided that the Prescript functionality was unlikely to be widely used and only introduced confusion when scripts executed. In some situations your prescript would execute, and in others it would never execute. It's gone.

#### Core database scripts

The existence of script libraries in core database was problematic to at least one of our users [375](https://github.com/SitecorePowerShell/Console/issues/375) and we've never really leveraged that capability in the platform either. As a result this functionality has been removed and if you've put any scripts there, you are now asked to kindly move them to either master database or one of the publishing target databases.


###### Happy Scripting!

###### // Michael and Adam

## Github issue lists for past releases
#### Version 3.1 (June 2015)
  [Release Notes](http://bit.ly/ScPs31Iss)
#### Version 3.0 (February 2015)
  [Release Notes](http://bit.ly/ScPs30Iss)
#### Version 2.8 (December 2014)
  [Release Notes](http://bit.ly/ScPs28Iss)
#### Version 2.7.5 (November 2014)
  [Release Notes](http://bit.ly/ScPs275Iss)
#### Version 2.7.1 (September 2014)
  [Release Notes](http://bit.ly/ScPs271Iss)
#### Version 2.7 (August 2014)
  [Release Notes](http://bit.ly/ScPs27Iss)
#### Version 2.6 (April 2014)
  [Release Notes](http://bit.ly/ScPs26Iss)
#### Version 2.5 (28 October 2013)
  [Release Notes](http://bit.ly/ScPs25Iss)
#### Version 2.4 (23 September 2013)
  [Release Notes](http://bit.ly/ScPs24Iss)
#### Version 2.3.1 (1 September 2013)
  [Release Notes](http://bit.ly/ScPs23Iss)
#### Version 2.2 (30 July 2013)
  [Release Notes](http://bit.ly/ScPs22Iss)
#### Version 2.1 (15 July 2013)
  [Release Notes](http://bit.ly/ScPs21Iss)

[1]: https://marketplace.sitecore.net/en/Modules/Sitecore_PowerShell_console.aspx
[2]: https://git.io/spe