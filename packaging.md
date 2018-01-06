# Packaging

Ever wanted to package up items and files without opening the Sitecore Package Designer each time? There are a number of commands available for generating packages.

**Example:** The following example demonstrates how to generate a package.

```powershell
$package = New-Package "Package of Stuff";
$package.Sources.Clear();

$package.Metadata.Author = "Michael West";
$package.Metadata.Publisher = "Team Awesome";
$package.Metadata.Version = "1.0";
$package.Metadata.Readme = @"
Set of instructions for the user.
"@

# Items using New-ItemSource and New-ExplicitItemSource
$source = Get-Item -Path "master:\templates\Feature\Forms" | 
    New-ItemSource -Name 'Feature Forms Items' -InstallMode Overwrite $package.Sources.Add($source)
$package.Sources.Add($source)

# Files using New-FileSource and New-ExplicitFileSource
$source = Get-Item -Path "$AppPath\App_Config\Include\Feature\Forms\Company.Feature.Forms.config" | 
    New-ExplicitFileSource -Name "Feature Forms Files"
$package.Sources.Add($source)

Export-Package -Project $package -Path "$($package.Name)-$($package.Metadata.Version).xml"
Export-Package -Project $package -Path "$($package.Name)-$($package.Metadata.Version).zip" -Zip
Download-File "$SitecorePackageFolder\$($package.Name)-$($package.Metadata.Version).zip"
```