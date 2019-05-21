# Get-UpdatePackageDiff

Performs a diff operation between the Source and taget path akin to Sitecore Courier. The diff is the difference that takes the content of Source folder and transforms it to Target. IMPORTANT! This functionality requires changes to web.config file on your sitecore server to work. Please consult the first Example.

## Syntax

Get-UpdatePackageDiff \[-SourcePath\] &lt;String&gt; \[\[-TargetPath\] &lt;String&gt;\]

## Detailed Description

Performs a diff operation between the Source and taget path akin to Sitecore Courier. The diff is the difference that takes the content of Source folder and transforms it to Target. IMPORTANT! This functionality requires changes to web.config file on your sitecore server to work. Please consult the first Example.

© 2010-2019 Adam Najmanowicz, Michael West. All rights reserved. Sitecore PowerShell Extensions

## Parameters

### -SourcePath  &lt;String&gt;

Path containing the current serialization items that needs to be transformed into Target.

| Aliases |  |
| :--- | :--- |
| Required? | true |
| Position? | 1 |
| Default Value |  |
| Accept Pipeline Input? | false |
| Accept Wildcard Characters? | false |

### -TargetPath  &lt;String&gt;

Path containing the desired serialization state that the Source needs to be transformed to.

| Aliases |  |
| :--- | :--- |
| Required? | false |
| Position? | 1 |
| Default Value |  |
| Accept Pipeline Input? | false |
| Accept Wildcard Characters? | false |

## Outputs

The output type is the type of the objects that the cmdlet emits.

* Sitecore.Update.Interfaces.ICommand 

## Notes

Help Author: Adam Najmanowicz, Michael West

## Examples

### EXAMPLE 1

Required addition to web.config file for the functionality to work:

```text
<configuration>
  <configSections>
    <section name="sitecorediff" type="Sitecore.Update.Configuration.ConfigReader, Sitecore.Update"/>
  </configSections>
  <sitecorediff>
    <commandfilters>
      <filter id="changedFieldsFilter" mode="on" type="Sitecore.Update.Commands.Filters.ChangedFieldsFilter, Sitecore.Update">
        <fields hint="list">
          <field>__Created</field>
          <field>{5DD74568-4D4B-44C1-B513-0AF5F4CDA34F}</field>
          <field>__Revision</field>
          <field>__Updated</field>
          <field>__Updated by</field>
        </fields>
      </filter>
    </commandfilters>
    <dataproviders>
      <dataprovider id="filesystemmain" type="Sitecore.Update.Data.Providers.FileSystemProvider, Sitecore.Update">
        <param>$(id)</param>
      </dataprovider>
      <dataprovider id="snapshotprovider" type="Sitecore.Update.Data.Providers.SnapShotProvider, Sitecore.Update">
        <param>$(id)</param>
      </dataprovider>
    </dataproviders>

    <source type="Sitecore.Update.Data.DataManager, Sitecore.Update">
      <param>source</param>
    </source>

    <target type="Sitecore.Update.Data.DataManager, Sitecore.Update">
      <param>target</param>
    </target>
  </sitecorediff>
</configuration>
```

### EXAMPLE 2

Create an update package that transforms the serialized database state defined in C:\temp\SerializationSource into into set defined in C:\temp\SerializationTarget

```text
$diff = Get-UpdatePackageDiff -SourcePath C:\temp\SerializationSource -TargetPath C:\temp\SerializationTarget
Export-UpdatePackage -Path C:\temp\SerializationDiff.update -CommandList $diff -Name name
```

## Related Topics

* [Export-UpdatePackage](export-updatepackage.md)
* [Install-UpdatePackage](install-updatepackage.md)
* [https://sitecoresnippets.blogspot.com/2012/10/sitecore-courier-effortless-packaging.html](https://sitecoresnippets.blogspot.com/2012/10/sitecore-courier-effortless-packaging.html) 
* [https://github.com/adoprog/Sitecore-Courier](https://github.com/adoprog/Sitecore-Courier) 
* [https://github.com/SitecorePowerShell/Console/](https://github.com/SitecorePowerShell/Console/) 

