# Community

There are some really amazing contributions and add-ons to SPE from the community.

## Unicorn

![SPE + Unicorn](https://user-images.githubusercontent.com/933163/50198867-4053b380-0313-11e9-9e46-5eb5513417ff.png)

A well known and widely adopted module [Unicorn](https://github.com/SitecoreUnicorn/Unicorn) has published some SPE commands. These commands are available \(and optional\) after installing Unicorn. Below are some samples ripped off from Kam Figy's blog posts [here](https://kamsar.net/index.php/2017/02/Unicorn-4-Preview-Part-2-SPE-Support/) and [here](https://kamsar.net/index.php/2017/02/Unicorn-4-Preview-Part-2-5-Generating-Packages-with-SPE/).

### Configurations

**Example:** The following lists configurations by name.

```text
# Default returns all configurations
Get-UnicornConfiguration

# Exact match
Get-UnicornConfiguration -Filter "Foundation.Foo"

# Filter using a wildcard
Get-UnicornConfiguration -Filter "Foundation.*"
```

### Syncing

**Example:** The following syncs configurations just like you would through the Unicorn Control Panel or the PowerShell API.

```text
# Sync one
Sync-UnicornConfiguration "Foundation.Foo"

# Sync multiple by name
Sync-UnicornConfiguration @("Foundation.Foo", "Foundation.Bar")

# Sync multiple from pipeline
Get-UnicornConfiguration "Foundation.*" | Sync-UnicornConfiguration

# Sync all, except transparent sync-enabled configurations
Get-UnicornConfiguration | Sync-UnicornConfiguration -SkipTransparent

# Optionally set log output level (Debug, Info, Warn, Error)
Sync-UnicornConfiguration -LogLevel Warn
```

![Example Syncing](https://user-images.githubusercontent.com/933163/50114210-9ccaac00-0209-11e9-9241-2738b50b1f75.png)

### Partial Syncing

```text
# Sync a single item (note: must be under Unicorn control)
Get-Item "/sitecore/content" | Sync-UnicornItem

# Sync multiple single items (note: all must be under Unicorn control)
Get-ChildItem "/sitecore/content" | Sync-UnicornItem 

# Sync an entire item tree, show only warnings and errors
Get-Item "/sitecore/content" | Sync-UnicornItem -Recurse -LogLevel Warn
```

### Reserializing

```text
# Reserialize one
Export-UnicornConfiguration "Foundation.Foo"

# Reserialize multiple by name
Export-UnicornConfiguration @("Foundation.Foo", "Foundation.Bar")

# Reserialize from pipeline
Get-UnicornConfiguration "Foundation.*" | Export-UnicornConfiguration
```

### Partial Reserializing

```text
# Reserialize a single item (note: must be under Unicorn control)
Get-Item "/sitecore/content" | Export-UnicornItem

# Reserialize multiple single items (note: all must be under Unicorn control)
Get-ChildItem "/sitecore/content" | Export-UnicornItem 

# Reserialize an entire item tree
Get-Item "/sitecore/content" | Export-UnicornItem -Recurse
```

### Converting to Raw Yaml

```text
# Convert an item to YAML format (always uses default excludes and field formatters)
Get-Item "/sitecore/content" | ConvertTo-RainbowYaml

# Convert many items to YAML strings
Get-ChildItem "/sitecore/content" | ConvertTo-RainbowYaml

# Disable all field formats and field filtering
# (e.g. disable XML pretty printing,
# and don't ignore the Revision and Modified fields, etc)
Get-Item "/sitecore/content" | ConvertTo-RainbowYaml -Raw
```

![Converting To Yaml](https://user-images.githubusercontent.com/933163/50114470-32663b80-020a-11e9-917c-6707e85524dd.png)

### Converting from Raw Yaml

```text
# Get IItemDatas from YAML variable
$rawYaml | ConvertFrom-RainbowYaml

# Get IItemData and disable all field filters
# (use this if you ran ConvertTo-RainbowYaml with -Raw)
$yaml | ConvertFrom-RainbowYaml -Raw
```

![Converting from Yaml](https://user-images.githubusercontent.com/933163/50114544-5cb7f900-020a-11e9-90a7-f5b834eb7285.png)

### Deserialization

```text
# Deserialize IItemDatas from ConvertFrom-RainbowYaml
$rawYaml | ConvertFrom-RainbowYaml | Import-RainbowItem

# Deserialize raw YAML from pipeline into Sitecore 
# Shortcut bypassing ConvertFrom-RainbowYaml
$yaml | Import-RainbowItem

# Deserialize and disable all field filters
# (use this if you ran ConvertTo-RainbowYaml with -Raw)
$yaml | Import-RainbowItem -Raw

# Deserialize multiple at once
$yamlStringArray | Import-RainbowItem

# Complete example that does nothing but eat CPU
Get-ChildItem "/sitecore/content" | ConvertTo-RainbowYaml | Import-RainbowItem
```

![Deserialization](https://user-images.githubusercontent.com/933163/50114603-8bce6a80-020a-11e9-8876-2df4d24e5443.png)

### Packaging

```text
# Create a new Sitecore Package (SPE cmdlet)
$pkg = New-Package -Name MyCustomPackage

# Get the Unicorn Configuration(s) we want to package
$configs = Get-UnicornConfiguration "Foundation.*" 

# Pipe the configs into New-UnicornItemSource 
# to process them and add them to the package project
# (without -Project, this would emit the source object(s) 
#   which can be manually added with $pkg.Sources.Add())
$configs | New-UnicornItemSource -Project $pkg

# Export the package to a zip file on disk
Export-Package -Project $pkg -Path "C:\foo.zip" -Zip
```

## SPE Modules

The following are Sitecore modules that enhance the SPE experience.

* [SPE Image Importer](https://marketplace.sitecore.net/en/Modules/S/SPE_Image_Uploader_Module10.aspx) : [Himadri Chakrabarti](https://twitter.com/himadric)
* [Multi-Item Publish](https://www.sitecorenutsbolts.net/2015/12/14/Multi-Item-Publish-with-Sitecore-Powershell-Extensions/) : [Richard Seal](https://twitter.com/rich_seal)
* [Publish Status](http://marketplace.sitecore.net/Modules/P/Publish_Status_for_Sitecore_Powershell_Extensions.aspx?sc_lang=en) : [Morten Engel](https://mortenengel.blogspot.com/2018/11/publish-viewercanceler-using-sitecore.html)
* [Westco Pull Up Fields](https://github.com/michaellwest/westco-spe-pullupfields) : [Michael West](https://twitter.com/MichaelWest101)
* [Westco Image Optimizer](https://github.com/michaellwest/westco-spe-imageoptimizer) : [Michael West](https://twitter.com/MichaelWest101)
