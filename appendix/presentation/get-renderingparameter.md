# Get-RenderingParameter

Gets the available rendering parameters found for a given rendering.

## Syntax

```text
Get-RenderingParameter -Instance <RenderingDefinition> [-Name <String[]>]
```

## Detailed Description

Gets the available rendering parameters found for a given rendering.

© 2010-2019 Adam Najmanowicz, Michael West. All rights reserved. Sitecore PowerShell Extensions

## Parameters

### -Instance  &lt;RenderingDefinition&gt;

Rendering definition containing the rendering parameters.

| Aliases |  |
| :--- | :--- |
| Required? | true |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | false |
| Accept Wildcard Characters? | false |

### -Name  &lt;String\[\]&gt;

The names (or keys) identifying specific rendering parameters.

| Aliases |  |
| :--- | :--- |
| Required? | false |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | false |
| Accept Wildcard Characters? | false |

## Inputs

The input type is the type of the objects that you can pipe to the cmdlet.

* Sitecore.Layouts.RenderingDefinition

## Outputs

The output type is the type of the objects that the cmdlet emits.

* IDictionary

## Notes

Help Author: Adam Najmanowicz, Michael West

## Examples

### EXAMPLE

The following gets all renderings for a given item and lists the available rendering parameters.

```text
$item = Get-Item -Path "master" -ID "{110D559F-DEA5-42EA-9C1C-8A5DF7E70EF9}"

foreach($itemRendering in Get-Rendering -Item $item -FinalLayout) {
    Write-Host "Rendering UniqueId: $($itemRendering.UniqueId)"
    Get-RenderingParameter -Rendering $itemRendering | Format-Table -Auto
}
```

## Related Topics

* [https://github.com/SitecorePowerShell/Console/](https://github.com/SitecorePowerShell/Console/) 
* [New-Rendering](new-rendering.md)
* [Set-Rendering](set-rendering.md)
* [Get-Rendering](get-rendering.md)
* [Get-LayoutDevice](get-layoutdevice.md)
* [Remove-Rendering](remove-rendering.md)
* [Get-Layout](get-layout.md)
* [Set-Layout](set-layout.md)

