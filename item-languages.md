# Item Languages

### Add Language Version

**Example:** The following example queries all of the content items and adds a new language version of "en-ca", while overwriting any that exist.

```powershell
Get-ChildItem "master:\content" -Recurse | 
    Add-ItemLanguage -Language "en-us" -TargetLanguage "en-ca" -IfExist OverwriteLatest
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