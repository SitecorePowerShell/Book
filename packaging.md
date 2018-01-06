# Packaging

Ever wanted to package up items and files without opening the Sitecore Package Designer each time? There are a number of commands available for generating packages.

```powershell
# Clear test items:
# Get-ChildItem 'master:\system\Modules\PowerShell\Script Library' -recurse | Where-Object {  ($_.Name -match '\(test\)') } | Remove-Item

$package = new-package "Sitecore PowerShell Extensions";
$package.Sources.Clear();

$package.Metadata.Author = "Adam Najmanowicz - Cognifide, Michael West";
$package.Metadata.Publisher = "Cognifide Limited";
$package.Metadata.Version = "2.6";
$package.Metadata.Readme = 'This module provides 2 immediately usable interfaces 
The "PowerShell Console" available from the "Sitecore"
menu and the "PowerShell ISE" available from the 
"Developer Tools" sub-menu.

PowerShell ISE comes with a sample script and
Script Library for you to experiment with. Enjoy!

Most Recent Releases:
- 2.6 (April 2014)
  Release Notes: http://bit.ly/ScPs26Iss
- 2.5 (28 October 2013)
  Release Notes: http://bit.ly/ScPs25Iss
- 2.4 (23 September 2013)
  Release Notes: http://bit.ly/ScPs24Iss
- 2.3.1 (1 September 2013)
  Release Notes: http://bit.ly/ScPs23Iss
- 2.2 (30 July 2013)
  Release Notes: http://bit.ly/ScPs22Iss
- 2.1 (15 July 2013)
  Release Notes: http://bit.ly/ScPs21Iss

Please report any problems in our issue tracker:
http://bit.ly/ScPsIss

Thank you for using Sitecore PowerShell Extensions.

Copyright (c) 2010-2013 Adam Najmanowicz - Cognifide
Copyright (c) 2013 Michael West

http://www.cognifide.com/
http://blog.najmanowicz.com/
http://michaellwest.blogspot.com/'

# Item templates
$source = Get-Item 'master:\templates\Modules\PowerShell Console' | New-ItemSource -Name 'Master Item Templates' -InstallMode Overwrite
$package.Sources.Add($source);
$source = Get-Item 'core:\templates\Modules\PowerShell Console' | New-ItemSource -Name 'Core Item Templates' -InstallMode Overwrite
$package.Sources.Add($source);

# Module Root
$source = Get-Item 'master:\system\Modules\PowerShell' | New-ExplicitItemSource -Name "Master Module Root" -InstallMode Merge -MergeMode Merge
$package.Sources.Add($source);
$source = Get-Item 'core:\system\Modules\PowerShell' | New-ExplicitItemSource -Name "Core Module Root" -InstallMode Merge -MergeMode Merge
$package.Sources.Add($source);

# Colors
$source = Get-Item 'master:\system\Modules\PowerShell\Console Colors' | New-ItemSource -Name "Master Colors" -InstallMode Overwrite
$package.Sources.Add($source);
$source = Get-Item 'core:\system\Modules\PowerShell\Console Colors' | New-ItemSource -Name "Core Module Root" -InstallMode Overwrite
$package.Sources.Add($source);

# Rules Engine Rules
$source = Get-Item 'master:\system\Modules\PowerShell\Rules' |  New-ItemSource -Name "Rules Engine" -InstallMode Merge -MergeMode Clear
$package.Sources.Add($source);

# Script Library
$source = Get-Item 'master:\system\Modules\PowerShell\Script Library' | New-ItemSource -Name "Master Script Library" -InstallMode Merge -MergeMode Clear
$package.Sources.Add($source);
$source = Get-Item 'core:\system\Modules\PowerShell\Script Library' | New-ItemSource -Name "Core Script Library" -InstallMode Merge -MergeMode Clear
$package.Sources.Add($source);

# Prepare settings - cleanup
$settingsToCleanup = get-childitem sitecore -Path master:\system\Modules\PowerShell\Settings\ -recurse | remove-item -recurse

# Settings
$source = Get-Item master:\system\Modules\PowerShell\Settings | New-ItemSource -Name "Settings" -InstallMode Merge -MergeMode Clear
$package.Sources.Add($source);

# Applications
$source = Get-Item core:\content\Applications\PowerShell | New-ItemSource -Name "Applications" -InstallMode Overwrite
$package.Sources.Add($source);

# Application Layouts
$source = Get-Item 'core:\layout\Layouts\Applications\*PowerShell*' | New-ExplicitItemSource "Application Layouts" -InstallMode Overwrite
$package.Sources.Add($source);

# Start Menu
$source = Get-ChildItem '*PowerShell*' -path 'core:/content/Documents and settings/All users/Start menu/'  -Recurse | New-ExplicitItemSource "Start Menu Icons" -InstallMode Overwrite
$package.Sources.Add($source);

# Content Editor Context Menu integration
$source = Get-ChildItem '*PowerShell*' -path 'core:/content/Applications/Content Editor/Context Menues/Default/'  -Recurse | New-ExplicitItemSource "Content Editor Context Menu integration #1" -InstallMode Merge -MergeMode Clear
$package.Sources.Add($source);
$source = Get-Item 'core:/content/Applications/Content Editor/Context Menues/Default/Edit Script/' | New-ExplicitItemSource "Content Editor Context Menu integration #2" -InstallMode Merge -MergeMode Clear
$package.Sources.Add($source);

# Content Editor Ribbon integration
$source = Get-Item 'core:/content/Applications/Content Editor/Ribbons/Chunks/PowerShell/' | New-ItemSource -Name "Content Editor Ribbon integration #1" -InstallMode Overwrite
$package.Sources.Add($source);
$source = Get-Item 'core:/content/Applications/Content Editor/Ribbons/Strips/View/PowerShell/' | New-ItemSource -Name "Content Editor Ribbon integration #2" -InstallMode Overwrite
$package.Sources.Add($source);

# Task Integration
$source = Get-Item master:\system\Tasks\Commands\PowerShellScriptCommand | New-ItemSource -Name "Task Integration - Command" -InstallMode Overwrite
$package.Sources.Add($source);
$source = Get-Item master:\system\Tasks\Schedules\Test-PowerShell | New-ItemSource -Name "Task Integration - Schedule" -InstallMode Skip
$package.Sources.Add($source);

# Files
$source = Get-Item "$AppPath\App_Config\Include\Cognifide.PowerShell.config" | New-ExplicitFileSource -Name "Configuration File"
$package.Sources.Add($source);
$source = Get-Item "$AppPath\bin\Cognifide.PowerShell.dll" | New-ExplicitFileSource -Name "PowerShell Binary"
$package.Sources.Add($source);
$source = Get-Item "$AppPath\sitecore/shell/Override/Controls/Splitters/HSplitter.xml" | New-ExplicitFileSource -Name "Splitter Fix"
$package.Sources.Add($source);
$source = Get-Item "$AppPath/sitecore/shell/Themes/Standard/PowerShell.zip" | New-ExplicitFileSource -Name "Icons"
$package.Sources.Add($source);
$source = Get-ChildItem -exclude *.cs -Path "$AppPath\Console" -Recurse -File | New-ExplicitFileSource -Name "Console Assets"
$package.Sources.Add($source);
$source = Get-ChildItem -Path "$AppPath\sitecore\Shell\Applications\Powershell\*" -Recurse -File | New-ExplicitFileSource -Name "Application Files"
$package.Sources.Add($source);

Export-Package -Project $package -Path "$($package.Name)-$($package.Metadata.Version).xml"
Export-Package -Project $package -Path "$($package.Name)-$($package.Metadata.Version).zip" -Zip
Download-File "$SitecorePackageFolder\$($package.Name)-$($package.Metadata.Version).zip"
```