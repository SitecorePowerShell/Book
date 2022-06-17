# Packaging

Ever wanted to package up items and files without opening the Sitecore Package Designer each time? There are a number of commands available for generating packages.

## Package Creation

**Example:** The following example demonstrates how to generate a package.

```powershell
$package = New-Package "Package-of-Stuff"
$package.Sources.Clear()

$package.Metadata.Author = "Michael West"
$package.Metadata.Publisher = "Team Awesome"
$package.Metadata.Version = "1.0"
$package.Metadata.Readme = @"
Set of instructions for the user.
"@

# Items using New-ItemSource and New-ExplicitItemSource
$source = Get-Item -Path "master:\templates\Feature\Forms" | 
    New-ItemSource -Name 'Feature Forms Items' -InstallMode Overwrite
$package.Sources.Add($source)

# Files using New-FileSource and New-ExplicitFileSource
$source = Get-Item -Path "$AppPath\App_Config\Include\Feature\Forms\Company.Feature.Forms.config" | 
    New-ExplicitFileSource -Name "Feature Forms Files"
$package.Sources.Add($source)

Export-Package -Project $package -Path "$($package.Name)-$($package.Metadata.Version).xml"
Export-Package -Project $package -Path "$($package.Name)-$($package.Metadata.Version).zip" -Zip
Download-File "$SitecorePackageFolder\$($package.Name)-$($package.Metadata.Version).zip"
```

## Post Step

**Example:** The following adds a Post Step and custom attributes.

```powershell
$package = New-Package "Package-of-Stuff"
$package.Sources.Clear()

$package.Metadata.Author = "Michael West"
$package.Metadata.Publisher = "Team Awesome"
$package.Metadata.Version = "1.0"
$package.Metadata.Readme = @"
Set of instructions for the user.
"@
$package.Metadata.PostStep = "Some.Library.Class,Some.Library"
$package.Metadata.Attributes = "scriptId={9b9a3906-1979-11e7-8c9d-177c30471cec}|width=50|height=200"

Export-Package -Project $package -Path "$($package.Name)-$($package.Metadata.Version).xml"
```

**Example:** The following adds a Post Step included with SPE to delete a file. The SPE Post Step code reads xml data stored in the comment section of the package.

```powershell
Import-Function -Name New-PackagePostStep

$package = New-Package "Package-of-Stuff"
$package.Sources.Clear()

$package.Metadata.Author = "Michael West"
$package.Metadata.Publisher = "Team Awesome"
$package.Metadata.Version = "1.0"
$package.Metadata.Readme = @"
Set of instructions for the user.
"@
$newPackageFiles = @([PSCustomObject]@{"FileName"="/bin/Company.Feature.Unused.dll"})
$package.Metadata.PostStep = "Spe.Package.Install.PackagePostStep, Spe.Package"
$package.Metadata.Comment = New-PackagePostStep -PackageFiles $newPackageFiles
```

**Example:** The following adds a Post Step Script included with SPE to change icons. The SPE Post Step code executes a script included with the package as stored in the attributes section.

```powershell
$package = New-Package "Package-of-Stuff"
$package.Sources.Clear()

$package.Metadata.Author = "Michael West"
$package.Metadata.Publisher = "Team Awesome"
$package.Metadata.Version = "1.0"
$package.Metadata.Readme = @"
Set of instructions for the user.
"@
$package.Metadata.PostStep = "Spe.Integrations.Install.ScriptPostStep, Spe"
$package.Metadata.Attributes = "scriptId={737CD0CC-12F7-4528-8FBD-E0FDEFC41325}"
```

```powershell
Write-Log "Processing changes to ensure backwards compatibility."
$oldVersion = New-Object System.Version(10,0)
if($PSVersionTable["SitecoreVersion"] -lt $oldVersion) {
    $iseButton = Get-Item -Path "core:{bfc79034-857c-4432-a5c2-2d93af784384}"
    $iseButton.Editing.BeginEdit()
    $iseButton.Fields["{D25B56D4-23B6-4462-BE25-B6A6D7F38E13}"].Value = "powershell/32x32/ise8.png"
    $iseButton.Editing.EndEdit() > $null

    $reportButton = Get-Item -Path "core:{74744022-353c-43f1-b8e4-5bc569ca9348}"
    $reportButton.Editing.BeginEdit()
    $reportButton.Fields["{D25B56D4-23B6-4462-BE25-B6A6D7F38E13}"].Value = "Office/32x32/chart_donut.png"
    $reportButton.Editing.EndEdit() > $null
    Write-Log "Changes complete."
} else {
    Write-Host "No changes required."
}
Close-Window
```
