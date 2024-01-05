---
description: Examples for managing complex field types such as MultilistField and NameValueListField.
---

# Field Types

## Edit MultilistField

**Example:** The following demonstrates how to set a field to a known list of Ids. The Id is already converted to a GUID string.

```powershell
# Hardcoded list of Ids.
$item.Editing.BeginEdit()
$item["Allowed Controls"] = "{guid1}|{guid2}|{guid3}"
$item.Editing.EndEdit()
```

```powershell
# Array of Ids.
$array = [System.Collections.ArrayList]@()
$array.Add({guid1}) > $null
$array.Add({guid2}) > $null
$ids = [System.String]::Join("|", $array)
$item.Editing.BeginEdit()
$item["Allowed Controls"] = $ids
$item.Editing.EndEdit()
```

**Example:** The following replaces an instance of an Id with an alternate Id. The Id is already converted to a GUID string.

```powershell
[Sitecore.Data.Fields.MultilistField]$field = $item.Fields["Allowed Controls"]
$item.Editing.BeginEdit()
$field.Replace("{493B3A83-0FA7-4484-8FC9-4680991CF742}","{493B3A83-0FA7-4484-8FC9-4680991CF743}")
$item.Editing.EndEdit()
```

**Example:** The following adds new Ids to an existing list. Makes use of the `Sitecore.Text.ListString` class.

```powershell
[Sitecore.Text.ListString]$ids = $item.Fields["Rendering"].Value
$ids.AddAt(0,"{guid1}") > $null
$ids.Add("{guid2}") > $null
$ids.Add("{guid3}") > $null

$item.Editing.BeginEdit()
$item.Fields["Rendering"].Value = $ids.ToString()
$item.Editing.EndEdit() > $null
```

**Example:** The following appends an `ID` to a set of items in all languages. It verifies that the field _Keywords_ exists.

```powershell
$items = Get-ChildItem -Path "master:\sitecore\content\home" -Recurse -Language *
foreach($item in $items) {
    if ($item.Keywords -and $item.Keywords.Length -gt 0) {
        $item.Keywords = $item.Keywords + "|{guid}"
    } else {
        $item.Keywords = "{guid}"
    }
}
```

**Example:** The following example gets all of the items of a `MultilistField` and append a specific `ID`, ensuring that it's delimited with the `|` character.

```powershell
$items = Get-ChildItem -Path "master:\sitecore\content\home" -Recurse -Language *
foreach($item in $items) {
    $item.Keywords = (@() + $item.Keywords.GetItems().ID + "{6D1EACDD-0DE7-4F3D-B55A-2CAE8EBFF3D0}" | Select-Object -Unique) -join "|"
}
```

**Example:** The following example extracts the items from a 'keywords' field, comma separates the values, and then outputs to a report.

```powershell
function Get-KeywordsAsString($item) {
    [Sitecore.Data.Fields.MultilistField] $field = $item.Fields["Keywords"]
    $items = $field.GetItems()
    $strArray = $items | Foreach { $_.DisplayName }
    $strArray -join ', '
}
Get-ChildItem -Path 'master:\content\home\path-to-item' `
  | Show-ListView -Property Name, Language, Version, ID, TemplateName, ItemPath, `
        @{ Label = "KeywordsAsString"; Expression = { Get-KeywordsAsString($_) } }
```

## Edit NameValueListField

**Example:** The following example gets all of the name/value pairs of a `NameValueListField` and appends a new pair.

```powershell
$item = Get-Item -Path "master:" -ID "{371EEE15-B6F3-423A-BB25-0B5CED860EEA}"

$nameValues = [System.Web.HttpUtility]::ParseQueryString($item.UrlMapping)

# Here you can add or remove name/value pairs
$nameValues["^/ab[cde]/$"] = "/somewhere/fun?lang=en"

foreach($key in $nameValues.AllKeys) {
    $nameValues[$key] = [Uri]::EscapeDataString($nameValues[$key])
}

$item.UrlMapping = [Sitecore.StringUtil]::NameValuesToString($nameValues,"&")
```
