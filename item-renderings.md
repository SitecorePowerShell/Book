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

**Example:** The following demonstrates how to report on pages referencing the specified rendering.

```powershell
Get-Item "master:\layout\Renderings\Feature\Experience Accelerator\Page Content\Page Content" | 
    Get-ItemReferrer | Where-Object { $_.ContentPath.StartsWith("/Demo/usa/Home") } | Show-ListView
```

**Example:** The following demonstrates how to report on which renderings are globally set to "Cacheable".

```powershell
Get-ChildItem -Path "master:\layout\Renderings" -Recurse | 
    Where-Object { $_.Cacheable -eq "1" } | 
    Select-Object -Property Name, Cacheable, ClearOnIndexUpdate, VaryBy* | 
    Sort-Object -Property Name | Show-ListView
```

**Example:** The following demonstrates how to disable global caching on all renderings.

```powershell
Get-ChildItem -Path "master:\layout\Renderings" -Recurse | 
    Where-Object { $_.Cacheable -eq "1" } | 
    ForEach-Object { Write-Verbose "Disabled global caching on $($_.Name)"; $_.Cacheable = "0" }
```


