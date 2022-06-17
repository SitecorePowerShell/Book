---
description: Useful code snippets to help you with those complex scripts.
---

# Code Snippets

## List fields on template

**Example:** The following demonstrates how to list all of the fields of a template excluding the Standard Template fields.

```powershell
# Create a list of field names on the Standard Template. This will help us filter out extraneous fields.
$standardTemplate = Get-Item -Path "master:" -ID "{1930BBEB-7805-471A-A3BE-4858AC7CF696}"
$standardTemplateTemplateItem = [Sitecore.Data.Items.TemplateItem]$standardTemplate
$standardFields = $standardTemplateTemplateItem.OwnFields + $standardTemplateTemplateItem.Fields | Select-Object -ExpandProperty key -Unique

$itemTemplate = Get-Item -Path "master:" -ID "{76036F5E-CBCE-46D1-AF0A-4143F9B557AA}"
$itemTemplateTemplateItem = [Sitecore.Data.Items.TemplateItem]$itemTemplate
$itemTemplateFields = $itemTemplateTemplateItem.OwnFields + $itemTemplateTemplateItem.Fields | Select-Object -ExpandProperty key -Unique

$filterFields = $itemTemplateFields | Where-Object { $standardFields -notcontains $_ } | Sort-Object
```

## Media item url

**Example:** The following demonstrates how to generate the public facing url from a media item.

```powershell
$item = Get-Item -Path "master:{04DAD0FD-DB66-4070-881F-17264CA257E1}"
$siteName = "website"

$site = [Sitecore.Sites.SiteContextFactory]::GetSiteContext($siteName)
New-UsingBlock (New-Object Sitecore.Sites.SiteContextSwitcher $site) {
    [Sitecore.Resources.Media.MediaManager]::GetMediaUrl($item)
}

# /-/media/default-website/cover.jpg
```

## Parse Html

**Example:** The following demonstrates the use of the **HtmlAgilityPack** for parsing html.

```powershell
$html = "<ul><li>foo</li><li>bar</li></ul>"
$htmlDocument = New-Object -TypeName HtmlAgilityPack.HtmlDocument
$htmlDocument.LoadHtml($html)
foreach($x in $htmlDocument.DocumentNode.SelectNodes("//li")) {
    $x.InnerText;
}
```

**Example:** The following demonstrates how to update text in the document and exclude certain nodes.

```powershell
$html = @"
<div class="kitchen">
   <div class="kitchen">
        <blockquote>kitchen<br />
            <span class="kitchen">kitchen</span>
        </blockquote>
        <a><img title="kitchen" src="https://kitchen-sink.local" /></a>
    </div>
</div>
"@

$htmlDocument = New-Object -TypeName HtmlAgilityPack.HtmlDocument
$htmlDocument.LoadHtml($html)
foreach($x in $htmlDocument.DocumentNode.Descendants()) {
    if($x.Name -ne "img" -and ![string]::IsNullOrEmpty($x.Text)) {
        $x.Text = $x.Text.Replace("kitchen", "sink")
    }
}

$htmlDocument.DocumentNode.OuterHtml
```

**Example:** The following demonstrates how to remove empty paragraph tags in an html field.

[Sitecore Stack Exchanage](https://sitecore.stackexchange.com/a/20845/95)

## Workflow History

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

## Restore Recycle bin items

**Example:** The following restores items in the media library that were removed yesterday. [Credit](https://gist.github.com/technomaz/58890edff903123083c77ad8f1b1b2e2) @technomaz.

```powershell
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

## Purge Recycle bin items

**Example:** The following will incrementally purge items from the recycle bin (master db) with a progress counter.

```powershell
$database = Get-Database -Name "master"
$archiveName = "recyclebin"
$archive = Get-Archive -Database $database -Name $archiveName
$items = Get-ArchiveItem -Archive $archive

$count = 0
$total = $items.Count
foreach($item in $items) {
    $count++
    if($count % 100 -eq 0) {
        Write-Host "[$(Get-Date -Format 'yyyy-MM-dd hh:mm:ss')] $([math]::round($count * 100 / $total, 2))% complete"
    }
    $item | Remove-ArchiveItem
}
Write-Host "Completed processing recycle bin"
```

## Run JavaScript

**Example:** The following logs messages to the browser console and then alerts the user with a message.

```powershell
1..5 | ForEach-Object { 
    Start-Sleep -Seconds 1
    Invoke-JavaScript -Script "console.log('Hello World! Call #$($_) from PowerShell...');" 
}

Invoke-JavaScript -Script "alert('hello from powershell');"
```

![Invoke JavaScript](../.gitbook/assets/interact-with-browser-using-js.gif)

## Remoting

**Example:** [Remote Package Installation](https://gist.github.com/michaellwest/14e9ef98f9e8b450c1b39813d13cbc50)

```powershell
Import-Module -Name SPE -Force

$packageName = "$($SitecorePackageFolder)\[PACKAGE].zip"

$session = New-ScriptSession -Username "admin" -Password "b" -ConnectionUri "https://remotesitecore"
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

