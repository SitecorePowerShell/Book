# Packaging

Ever wanted to package up items and files without opening the Sitecore Package Designer each time? There are a number of commands available for generating packages.

## Package Creation

**Example:** The following example demonstrates how to generate a package.

```text
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

```text
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

**Example:** The following adds a Post Step included with SPE to delete a file.

```text
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

The function `New-PackagePostStep` used by the example above can be included in the same script. 
```text
function New-PackagePostStep {
    param(
        $PackageItems,
        $PackageFiles
    )
    
    $writer = New-Object System.IO.StringWriter
    $output = New-Object System.Xml.XmlTextWriter([System.IO.TextWriter]$writer)
    $output.Formatting = [System.Xml.Formatting]::Indented
    $output.WriteStartElement("uninstall")
    
    if($PackageItems) {
        $output.WriteStartElement("items")
        foreach($packageItem in $PackageItems) {
            $output.WriteStartElement("item")
            $output.WriteAttributeString("database", $packageItem.Database)
            $output.WriteAttributeString("id", $packageItem.ID.ToString())
            $output.WriteEndElement()
        }
        $output.WriteEndElement()
    }
    if($PackageFiles) {
        $output.WriteStartElement("files")
        foreach($packageFile in $PackageFiles) {
            $output.WriteStartElement("file")
            $output.WriteAttributeString("filename", $packageFile.FileName)
            $output.WriteEndElement()
        }
        $output.WriteEndElement()            
    }

    $output.WriteEndElement()
    $writer.ToString()
}
```

