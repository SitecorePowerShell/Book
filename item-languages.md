# Item Languages

### Add Language Version

**Example:** The following example queries all of the content items and adds a new language version of "en-ca", while overwriting any that exist.

```powershell
Get-ChildItem "master:\content" -Recurse | 
    Add-ItemLanguage -Language "en-us" -TargetLanguage "en-ca" -IfExist OverwriteLatest
```

**Example:** The following example adds a language version from English to US and Polish while leaving the *Title* field blank. If a version already exists nothing happens.

```powershell
$languageParameters = @{
    Path = "master:\content\home"
    Language = "en"
    TargetLanguage = @("pl-pl","en-us")
    IfExist = "Skip"
    IgnoredFields = @("Title")
}
Add-ItemLanguage @languageParameters
```

**Example:** The following example adds a language version from English to Polish of Template Name *Sample Item*. If the version exists a new version is created for that language. Finally the results are displayed as a table showing only the *Name*, *Language*, and *Version*.
```powershell
Get-ChildItem "master:\content\home" -Language "en" -Recurse |
    Where-Object { $_.TemplateName -eq "Sample Item" } |
    Add-ItemLanguage -TargetLanguage "pl-pl" -IfExist Append |
    Format-Table Name, Language, Version -AutoSize
```

### Remove Language Version

**Example:** The following example queries all of the content items and removes the language version of "fr-CA".

```powershell
Get-ChildItem "master:\content" -Recurse | 
    Remove-ItemLanguage -Language "fr-CA"
```

#### References
* [Issue 184](https://github.com/SitecorePowerShell/Console/issues/184)
* [Remove All Content items in French](http://stackoverflow.com/questions/29928540/powershell-script-to-remove-all-content-items-for-french-version-in-sitecore)