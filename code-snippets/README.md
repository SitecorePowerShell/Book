# Code Snippets

## Change Template

```text
# Sample Item
$sourceTemplate = Get-Item -Path "master:\{76036F5E-CBCE-46D1-AF0A-4143F9B557AA}"
# Sample Content
$targetTemplate = Get-Item -Path "master:\{93A8866B-972F-4FBF-8FD9-D6004B18C0AF}"

# Use Get-ItemReferrer to find all items referencing the template, rather than scanning the content tree.
$sourceTemplate | Get-ItemReferrer | 
    Where-Object { $PSItem.TemplateId -eq $sourceTemplate.ID -and $PSItem.Paths.IsContentItem } |
    ForEach-Object {
        Set-ItemTemplate -Item $PSItem -TemplateItem $targetTemplate
    }
```

## Edit MultilistField

**Example:** The following demonstrates how to set a field to a known list of Ids. The Id is already converted to a GUID string.

```text
# Hardcoded list of Ids.
$item.Editing.BeginEdit()
$item["Allowed Controls"] = "{guid1}|{guid2}|{guid3}"
$item.Editing.EndEdit()
```

```text
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

```text
[Sitecore.Data.Fields.MultilistField]$field = $item.Fields["Allowed Controls"]
$item.Editing.BeginEdit()
$field.Replace("{493B3A83-0FA7-4484-8FC9-4680991CF742}","{493B3A83-0FA7-4484-8FC9-4680991CF743}")
$item.Editing.EndEdit()
```

**Example:** The following adds new Ids to an existing list. Makes use of the `Sitecore.Text.ListString` class.

```text
[Sitecore.Text.ListString]$ids = $item.Fields["Rendering"].Value
$ids.AddAt(0,"{guid1}") > $null
$ids.Add("{guid2}") > $null
$ids.Add("{guid3}") > $null

$item.Editing.BeginEdit()
$item.Fields["Rendering"].Value = $ids.ToString()
$item.Editing.EndEdit() > $null
```

**Example:** The following appends a an `ID` to a set of items in all languages. It verifies that the field _Keywords_ exists.

```text
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

```text
$items = Get-ChildItem -Path "master:\sitecore\content\home" -Recurse -Language *
foreach($item in $items) {
    $item.Keywords = (@() + $item.Keywords.GetItems().ID + "{6D1EACDD-0DE7-4F3D-B55A-2CAE8EBFF3D0}" | Select-Object -Unique) -join "|"
}
```

## Edit NameValueListField

**Example:** The following example gets all of the name/value pairs of a `NameValueListField` and appends a new pair.

```text
$item = Get-Item -Path "master:" -ID "{371EEE15-B6F3-423A-BB25-0B5CED860EEA}"

$nameValues = New-Object System.Collections.Specialized.NameValueCollection
$pairs = $item.Redirects -split "&"
foreach($pair in $pairs) {
    $split = $pair -split "="
    if($split -and $split.Length -le 2) {
        Write-Host "$($split[0]) = $($split[1])"
        $nameValues.Add($split[0],$split[1])
    }
}
# Here you can add or remove name/value pairs
$nameValues["^/ab[cd]/$"] = "/somewhere/fun"
$item.Redirects = [Sitecore.StringUtil]::NameValuesToString($nameValues,"&")
```

## Parse Html

**Example:** The following demonstrates the use of the **HtmlAgilityPack** for parsing html.

```text
$html = "<ul><li>foo</li><li>bar</li></ul>"
$htmlDocument = New-Object -TypeName HtmlAgilityPack.HtmlDocument
$htmlDocument.LoadHtml($html)
foreach($x in $htmlDocument.DocumentNode.SelectNodes("//li")) {
    $x.InnerText;
}
```

## Workflow History

**Example:** The following prints the workflow history of the home item.

```text
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

## Restore Recycle bin items

**Example:** The following restores items in the media library that were removed yesterday. [Credit](https://gist.github.com/technomaz/58890edff903123083c77ad8f1b1b2e2) @technomaz.

```text
Write-Host "Restoring items recycled after $($archivedDate.ToShortDateString())"

foreach($archive in Get-Archive -Name "recyclebin") {
    Write-Host " - Found $($archive.GetEntryCount()) entries"
    $entries = $archive.GetEntries(0, $archive.GetEntryCount())
    foreach($entry in $entries) {
        if($entry.ArchiveLocalDate -ge $archivedDate) { 
            Write-Host "Restoring item: $($entry.OriginalLocation) {$($entry.ArchivalId)}on date $($entry.ArchiveLocalDate)"
            $archive.RestoreItem($entry.ArchivalId)
        } else {
            Write-Host "Skipping $($entry.OriginalLocation) on date $($entry.ArchiveLocalDate)"
        }
    }
}
```

## Run JavaScript

**Example:** The following logs messages to the browser console and then alerts the user with a message.

```text
1..5 | ForEach-Object { 
    Start-Sleep -Seconds 1
    Invoke-JavaScript -Script "console.log('Hello World! Call #$($_) from PowerShell...');" 
}

Invoke-JavaScript -Script "alert('hello from powershell');"
```

![Invoke JavaScript](../.gitbook/assets/interact-with-browser-using-js.gif)

**Example:** [Remote Package Installation](https://gist.github.com/michaellwest/14e9ef98f9e8b450c1b39813d13cbc50)

```text
Import-Module -Name SPE -Force

$packageName = "$($SitecorePackageFolder)\[PACKAGE].zip"

$session = New-ScriptSession -Username "admin" -Password "b" -ConnectionUri "http://remotesitecore"
Test-RemoteConnection -Session $session -Quiet
$jobId = Invoke-RemoteScript -Session $session -ScriptBlock {
    [Sitecore.Configuration.Settings+Indexing]::Enabled = $false
    Get-SearchIndex | ForEach-Object { Stop-SearchIndex -Name $_.Name }
    Import-Package -Path "$($SitecorePackageFolder)\$($using:packageName)" -InstallMode Merge -MergeMode Merge
    [Sitecore.Configuration.Settings+Indexing]::Enabled = $true
} -AsJob
Wait-RemoteScriptSession -Session $session -Id $jobId -Delay 5 -Verbose
Stop-ScriptSession -Session $session
```

Not seeing what you are looking for? You can always check out some Github Gists that [Adam](https://gist.github.com/adamnaj) and [Michael](https://gist.github.com/michaellwest) have shared or the [Sitecore Stack Exchange](https://sitecore.stackexchange.com/questions/tagged/powershell-extensions).

