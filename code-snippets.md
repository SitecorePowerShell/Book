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
