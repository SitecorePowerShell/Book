### Change Template

```powershell
# Sample Item
$sourceTemplate = Get-Item -Path "master:\{76036F5E-CBCE-46D1-AF0A-4143F9B557AA}"
# Sample Content
$targetTemplate = Get-Item -Path "master:\{93A8866B-972F-4FBF-8FD9-D6004B18C0AF}"

# Use Get-ItemReferrer to find all items referencing the template, rather than scanning the content tree.
$sourceTemplate | Get-ItemReferrer | Where-Object { $PSItem.Paths.IsContentItem } |
    ForEach-Object {
        Set-ItemTemplate -Item $PSItem -TemplateItem $targetTemplate
    }
```

### Edit MultilistField

**Example:** The following demonstrates how to set a field to a known list of Ids. The Id is already converted to a GUID string.

```powershell
# Hardcoded list of Ids.
$item.Editing.BeginEdit()
$item["Allowed Controls"] = "{guid1}|{guid2}|{guid3}"
$item.Editing.EndEdit()

# Array of Ids.
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

**Example:** The following appends a an `ID` to a set of items in all languages. It verifies that the field *Keywords* exists.

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

### Parse Html

**Example:** The following demonstrates the use of the **HtmlAgilityPack** for parsing html.

```powershell
$html = "<ul><li>foo</li><li>bar</li></ul>"
$htmlDocument = New-Object -TypeName HtmlAgilityPack.HtmlDocument
$htmlDocument.LoadHtml($html)
foreach($x in $htmlDocument.DocumentNode.SelectNodes("//li")) {
    $x.InnerText;
}
```

### Workflow History
**Example:** The following prints the workflow history of the home item.

```powershell
$item = Get-Item -Path "master:" -Id "{110D559F-DEA5-42EA-9C1C-8A5DF7E70EF9}"

$db = Get-Database -Name "master"
$workflowProvider = $db.WorkflowProvider

foreach($version in $item.Versions.GetVersions()) {
    $workflowEvents = $workflowProvider.HistoryStore.GetHistory($version)
    foreach($workflowEvent in $workflowEvents) {
        "[$($workflowEvent.Date)] ($($workflowEvent.User)) $(($workflowEvent.Text -replace '(\r|\n)',''))"
    }
}
```

### Run JavaScript

**Example:** The following logs messages to the browser console and then alerts the user with a message.

```powershell
1..5 | ForEach-Object { 
    Start-Sleep -Seconds 1
    Invoke-JavaScript -Script "console.log('Hello World! Call #$($_) from PowerShell...');" 
}

Invoke-JavaScript -Script "alert('hello from powershell');"
```
![Invoke JavaScript](images/screenshots/code-snippets/interact-with-browser-using-js.gif)

**Gist:** Template Complexity Analysis

{% gist id="https://gist.github.com/AdamNaj/035366c698ef98e1b00a574eb085e790" %}{% endgist %}

**Gist:** Remote Package Installation

{% gist id="https://gist.github.com/michaellwest/14e9ef98f9e8b450c1b39813d13cbc50" %}{% endgist %}

Not seeing what you are looking for? You can always check out some Github Gists that [Adam][1] and [Michael][2] have shared.

[1]: https://gist.github.com/adamnaj
[2]: https://gist.github.com/michaellwest
