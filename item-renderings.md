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
$VerbosePreference = "Continue"
Get-ChildItem -Path "master:\layout\Renderings" -Recurse | 
    Where-Object { $_.Cacheable -eq "1" } | 
    ForEach-Object { Write-Verbose "Disabled global caching on $($_.Name)"; $_.Cacheable = "0" }

# VERBOSE: Disabled global caching on Navigation
```

**Example:** The following moves renderings from one placeholder to another. [See this article for more details](https://www.kasaku.co.uk/2018/02/28/updating-rendering-placeholders/).

```powershell
$placeholderMappings = @(
 @("/old-placeholder","/new-placeholder"),
 @("/another-old-placeholder","/new-placeholder")
)
$rootItem = Get-Item -Path master:/sitecore/content/Home
$defaultLayout = Get-LayoutDevice "Default"
# Toggle for whether to update Shared or Final Layout
$useFinalLayout = $True
# If set to true, the script will only list the renderings that need fixing, rather than fixing them.
$reportOnly = $False
foreach ( $item in Get-ChildItem -Item $rootItem -Recurse )
{
    # Only interested in items that have a layout
    if (Get-Layout $item)
    {
        foreach( $mapping in $placeholderMappings )
        {
            # Get renderings in this item that have renderings in the placeholder we want to update 
            $renderings =  Get-Rendering -Item $item -Placeholder ($mapping[0] + '/*') -Device $defaultLayout -FinalLayout:$useFinalLayout
            
            foreach ( $rendering in $renderings )
            {
                # Only update the rendering if we're not in "Report Only" mode
                if (!$reportOnly)
                {
                   # Update the placeholder in the rendering and set it back in the item
                   $rendering.Placeholder = $rendering.Placeholder -replace $mapping[0], $mapping[1]
                   Set-Rendering -Item $item -Instance $rendering -FinalLayout:$useFinalLayout
                }
                Write-Host "$($item.FullPath) - Rendering $($rendering.UniqueID) - Placeholder: $($mapping[0]) --> $($mapping[1])"
            }
        }
    }
}
```

**Example:** The following copies or moves renderings from one device to another (within Shared layout).

{% gist id="https://gist.github.com/AdamNaj/8680921177c338e210f90bd538a7c917" %}{% endgist %}




