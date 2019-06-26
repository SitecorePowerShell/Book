# Item Languages

The section on [working with items](./) provided a variety of examples in retrieving items based on their language. In this section will show how to manage items and their languages.

## Add Language Version

**Example:** The following example queries all of the content items and adds a new language version of "en-ca", while overwriting any that exist.

```text
Get-ChildItem "master:\content" -Recurse | 
    Add-ItemLanguage -Language "en-us" -TargetLanguage "en-ca" -IfExist OverwriteLatest
```

**Example:** The following example adds a language version from English to US and Polish while leaving the `Title` field blank. If a version already exists nothing happens.

```text
$languageParameters = @{
    Path = "master:\content\home"
    Language = "en"
    TargetLanguage = @("pl-pl","en-us")
    IfExist = "Skip"
    IgnoredFields = @("Title")
}
Add-ItemLanguage @languageParameters
```

**Example:** The following example adds a language version from English to Polish of Template Name _Sample Item_. If the version exists a new version is created for that language. Finally the results are displayed as a table showing only the `Name`, `Language`, and `Version`.

```text
Get-ChildItem "master:\content\home" -Language "en" -Recurse |
    Where-Object { $_.TemplateName -eq "Sample Item" } |
    Add-ItemLanguage -TargetLanguage "pl-pl" -IfExist Append |
    Format-Table Name, Language, Version -AutoSize
```

**Example:** The following example adds a language version in Polish to the _Home_ item and all its children. If the version exists nothing happens. No fields were harmed in the making of this version.

```text
Add-ItemLanguage -Path "master:\content\home" -TargetLanguage "pl-pl" -IfExist Skip -DoNotCopyFields -Recurse
```

## Remove Language Version

**Example:** The following example queries all of the content items and removes the language version of "fr-CA".

```text
Get-ChildItem "master:\content" -Recurse | 
    Remove-ItemLanguage -Language "fr-CA"
```

## New Item with Forced Language Version

**Example:** The following example creates a new item with language versions only matching the specified languages; all other language version are removed.

```text
$itemPath = "master:\content\home\sample item\Sample Item 3"
New-Item -Path $itemPath -ItemType "Sample/Sample Item" -Language "en-CA"

Name          Children Language Version Id                                     TemplateName
----          -------- -------- ------- --                                     ------------
Sample Item 3 False    en-CA    1       {C9517583-3AF9-4AFB-B247-BB0A09F55D94} Sample Item
```

## Parameters and Configuration

Supported parameters:

* `-Recurse` Translates item and its children
* `-IfExist` Accepts one of 3 pretty self explanatory actions: `Skip`, `Append` or `OverwriteLatest`
* `-TargetLanguage` accepts a list of languages that should be created
* `-DoNotCopyFields` creates a new version but does not copy field values from original language
* `-IgnoredFields` list of fields that should not be copied over from original item this can contain e.g. `__Security` if you don't want the new version to have the same restrictions as the original version.

On top of the ignored fields in the `-IgnoredFields` the following fields are ignored as configured within the `Cognifide.PowerShell.config` file:

```markup
<configuration xmlns:patch="https://www.sitecore.net/xmlconfig/">
  <sitecore>
    <powershell>
      <translation>
        <ignoredFields>
          <field>__Archive date</field>
          <field>__Archive Version date</field>
          <field>__Lock</field>
          <field>__Owner</field>
          <field>__Page Level Test Set Definition</field>
          <field>__Reminder date</field>
          <field>__Reminder recipients</field>
          <field>__Reminder text</field>
          <!--field>__Security</field-->
        </ignoredFields>
      </translation>
  </sitecore>
</configuration>
```

### References

* [Issue 184](https://github.com/SitecorePowerShell/Console/issues/184)
* [Remove All Content items in French](https://stackoverflow.com/questions/29928540/powershell-script-to-remove-all-content-items-for-french-version-in-sitecore)

