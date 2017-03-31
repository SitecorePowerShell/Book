# Item Renderings

In this section we'll show how to manage item renderings.

**Example:** The following demonstrates the use of `Get-Rendering` and `Set-Rendering` for updating values on templates.

```powershell
$rendering = Get-Item -Path "master:\sitecore\layout\Sublayouts\Sample Sublayout"

$items = Get-ChildItem -Path "master:\sitecore\templates\Sample Item" -Recurse 
foreach($item in $items) {
    $renderingInstance = Get-Rendering -Item $_ -Rendering $rendering 
    if ($renderingInstance) { 
        Set-Rendering -Item $_ -Instance $renderingInstance -Parameter @{ 
            "Lorem" = "Ipsum" 
        } 
        Write-Host "Updated $($_.Paths.FullPath)" 
    } 
}
```
