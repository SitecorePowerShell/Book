# Item Languages

**Example:** The following example queries all of the content items and adds a new language version of "en-ca", while overwriting any that exist.

```powershell
Get-ChildItem "master:\content" -Recurse | 
    Add-ItemLanguage -Language "en-us" -TargetLanguage "en-ca" -IfExist OverwriteLatest
```

#### References
* [Issue 184](https://github.com/SitecorePowerShell/Console/issues/184)