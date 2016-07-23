**Example:** The following demonstrates the use of the HtmlAgilityPack for parsing html.

```powershell
$html = "<ul><li>foo</li><li>bar</li></ul>"
$htmlDocument = New-Object -TypeName HtmlAgilityPack.HtmlDocument
$htmlDocument.LoadHtml($html)
foreach($x in $htmlDocument.DocumentNode.SelectNodes("//li")) {
    $x.InnerText;
}
```

Not seeing what you are looking for? You can always check out some Github Gists that [Adam][1] and [Michael][2] have shared.

[1]: https://gist.github.com/adamnaj
[2]: https://gist.github.com/michaellwest
