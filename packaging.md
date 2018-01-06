# Packaging

Ever wanted to package up items and files without opening the Sitecore Package Designer each time? There are a number of commands available for generating packages.

```powershell
$package = New-Package "Package of Stuff";
$package.Sources.Clear();

$package.Metadata.Author = "Michael West";
$package.Metadata.Publisher = "Team Awesome";
$package.Metadata.Version = "1.0";
$package.Metadata.Readme = @"
Set of instructions for the user.
"@

# Dynamic Items
$source = Get-Item 'master:\templates\Modules\PowerShell Console' | New-ItemSource -Name 'Master Item Templates' -InstallMode Overwrite
$package.Sources.Add($source)

# Specific Items
$source = Get-Item 'master:\system\Modules\PowerShell' | New-ExplicitItemSource -Name "Master Module Root" -InstallMode Merge -MergeMode Merge
$package.Sources.Add($source)

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