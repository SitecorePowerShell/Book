# Item Renderings

In this section we'll show how to manage item renderings.

### Update rendering parameters

**Example:** The following demonstrates the use of `Get-Rendering` and `Set-Rendering` for updating values on templates.

```text
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

### Find pages using rendering

**Example:** The following demonstrates how to report on pages referencing the specified rendering.

```text
Get-Item "master:\layout\Renderings\Feature\Experience Accelerator\Page Content\Page Content" | 
    Get-ItemReferrer | Where-Object { $_.ContentPath.StartsWith("/Demo/usa/Home") } | Show-ListView
```

### Find renderings marked cacheable

**Example:** The following demonstrates how to report on which renderings are globally set to "Cacheable".

```text
Get-ChildItem -Path "master:\layout\Renderings" -Recurse | 
    Where-Object { $_.Cacheable -eq "1" } | 
    Select-Object -Property Name, Cacheable, ClearOnIndexUpdate, VaryBy* | 
    Sort-Object -Property Name | Show-ListView
```

### Find renderings with personalization

**Example:** The following demonstrates how to find renderings with a conditions node set on the item.

```text
$query = "fast:/sitecore/content//*[@__renderings='%<conditions%' or @#__Final Renderings#='%<conditions%']"
$items = Get-Item -Path "master:" -Query $query
```

### Disable cacheable setting on renderings

**Example:** The following demonstrates how to disable global caching on all renderings.

```text
$VerbosePreference = "Continue"
Get-ChildItem -Path "master:\layout\Renderings" -Recurse | 
    Where-Object { $_.Cacheable -eq "1" } | 
    ForEach-Object { Write-Verbose "Disabled global caching on $($_.Name)"; $_.Cacheable = "0" }

# VERBOSE: Disabled global caching on Navigation
```

### Move renderings between placeholders

**Example:** The following moves renderings from one placeholder to another. [See this article for more details](https://www.kasaku.co.uk/2018/02/28/updating-rendering-placeholders/).

```text
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

### Remove datasource from rendering

**Example:** The following removes a datasource from a rendering on the FinalLayout.

```text
Get-Rendering -Item $item -PlaceHolder "main" | 
  Foreach-Object { Set-Rendering -Item $item -Instance $_ -DataSource $null -FinalLayout }
```

### Replace compatible rendering

```text
$rendering = Get-Item master:\layout\path\to\your\rendering
$renderingPageContainer = Get-Rendering -Item $item "{F39BAC93-1EEC-446B-A4A1-AB7F7C1B6267}" -Device $defaultLayout
$renderingPageContainer.ItemID = $rendering.ID
Set-Rendering -Item $item -Instance $renderingPageContainer
```

### Further Reading

* [Parse raw layout xml and count components](https://sitecore.stackexchange.com/questions/4952/get-amount-of-components-on-final-layout-programmatically)



