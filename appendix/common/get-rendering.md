# Get-Rendering

Returns a RenderingDefinition for an item using the filtering parameters.

## Syntax

Get-Rendering -Item &lt;Item&gt; \[-DataSource &lt;String&gt;\] \[-Rendering &lt;Item&gt;\] \[-Index &lt;Int32&gt;\] \[-PlaceHolder &lt;String&gt;\] \[-Parameter &lt;Hashtable&gt;\] \[-Device &lt;DeviceItem&gt;\] \[-FinalLayout\] \[-Language &lt;String\[\]&gt;\]

Get-Rendering -Item &lt;Item&gt; -Instance &lt;RenderingDefinition&gt; \[-Device &lt;DeviceItem&gt;\] \[-FinalLayout\] \[-Language &lt;String\[\]&gt;\]

Get-Rendering -Item &lt;Item&gt; -UniqueId &lt;String&gt; \[-Device &lt;DeviceItem&gt;\] \[-FinalLayout\] \[-Language &lt;String\[\]&gt;\]

Get-Rendering -Path &lt;String&gt; \[-DataSource &lt;String&gt;\] \[-Rendering &lt;Item&gt;\] \[-Index &lt;Int32&gt;\] \[-PlaceHolder &lt;String&gt;\] \[-Parameter &lt;Hashtable&gt;\] \[-Device &lt;DeviceItem&gt;\] \[-FinalLayout\] \[-Language &lt;String\[\]&gt;\]

Get-Rendering -Path &lt;String&gt; -Instance &lt;RenderingDefinition&gt; \[-Device &lt;DeviceItem&gt;\] \[-FinalLayout\] \[-Language &lt;String\[\]&gt;\]

Get-Rendering -Path &lt;String&gt; -UniqueId &lt;String&gt; \[-Device &lt;DeviceItem&gt;\] \[-FinalLayout\] \[-Language &lt;String\[\]&gt;\]

Get-Rendering \[-Id &lt;String&gt;\] \[-Database &lt;String&gt;\] \[-DataSource &lt;String&gt;\] \[-Rendering &lt;Item&gt;\] \[-Index &lt;Int32&gt;\] \[-PlaceHolder &lt;String&gt;\] \[-Parameter &lt;Hashtable&gt;\] \[-Device &lt;DeviceItem&gt;\] \[-FinalLayout\] \[-Language &lt;String\[\]&gt;\]

Get-Rendering \[-Id &lt;String&gt;\] \[-Database &lt;String&gt;\] -Instance &lt;RenderingDefinition&gt; \[-Device &lt;DeviceItem&gt;\] \[-FinalLayout\] \[-Language &lt;String\[\]&gt;\]

Get-Rendering \[-Id &lt;String&gt;\] \[-Database &lt;String&gt;\] -UniqueId &lt;String&gt; \[-Device &lt;DeviceItem&gt;\] \[-FinalLayout\] \[-Language &lt;String\[\]&gt;\]

## Detailed Description

The Get-Rendering command returns a RenderingDefinition for an item using the filtering parameters.

© 2010-2017 Adam Najmanowicz, Michael West. All rights reserved. Sitecore PowerShell Extensions

## Parameters

### -Item  &lt;Item&gt;

The item to be processed.

| Aliases |  |
| :--- | :--- |
| Required? | true |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | true \(ByValue, ByPropertyName\) |
| Accept Wildcard Characters? | false |

### -Path  &lt;String&gt;

Path to the item to be processed - can work with Language parameter to narrow the publication scope.

| Aliases |  |
| :--- | :--- |
| Required? | true |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | false |
| Accept Wildcard Characters? | false |

### -Id  &lt;String&gt;

Id of the item to be processed - can work with Language parameter to narrow the publication scope.

| Aliases |  |
| :--- | :--- |
| Required? | false |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | false |
| Accept Wildcard Characters? | false |

### -Database  &lt;String&gt;

Database containing the item to be processed - can work with Language parameter to narrow the publication scope.

| Aliases |  |
| :--- | :--- |
| Required? | false |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | false |
| Accept Wildcard Characters? | false |

### -DataSource  &lt;String&gt;

Data source filter - supports wildcards.

| Aliases |  |
| :--- | :--- |
| Required? | false |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | false |
| Accept Wildcard Characters? | false |

### -Rendering  &lt;Item&gt;

Item representing the sublayout/rendering. If matching the rendering will be returned.

| Aliases |  |
| :--- | :--- |
| Required? | false |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | false |
| Accept Wildcard Characters? | false |

### -Index  &lt;Int32&gt;

Index at which the rendering exists in the layout. The rendering at that index will be returned.

| Aliases |  |
| :--- | :--- |
| Required? | false |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | false |
| Accept Wildcard Characters? | false |

### -PlaceHolder  &lt;String&gt;

Place holder at which the rendering exists in the layout. Renderings at that place holder will be returned.

| Aliases |  |
| :--- | :--- |
| Required? | false |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | false |
| Accept Wildcard Characters? | false |

### -Parameter  &lt;Hashtable&gt;

Additional rendering parameter values. If both name and value match - the rendering will be returned. Values support wildcards.

| Aliases |  |
| :--- | :--- |
| Required? | false |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | false |
| Accept Wildcard Characters? | false |

### -Instance  &lt;RenderingDefinition&gt;

Specific instance of rendering that should be returned. The instance could earlier be obtained through e.g. use of Get-Rendering.

| Aliases |  |
| :--- | :--- |
| Required? | true |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | false |
| Accept Wildcard Characters? | false |

### -UniqueId  &lt;String&gt;

UniqueID of the rendering to be retrieved.

| Aliases |  |
| :--- | :--- |
| Required? | true |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | false |
| Accept Wildcard Characters? | false |

### -Device  &lt;DeviceItem&gt;

Device for which the renderings will be retrieved.

| Aliases |  |
| :--- | :--- |
| Required? | false |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | false |
| Accept Wildcard Characters? | false |

### -FinalLayout  &lt;SwitchParameter&gt;

Targets the Final Layout. If not provided, the Shared Layout will be targeted. Applies to Sitecore 8.0 and higher only.

| Aliases |  |
| :--- | :--- |
| Required? | false |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | false |
| Accept Wildcard Characters? | false |

### -Language  &lt;String\[\]&gt;

| Aliases |  |
| :--- | :--- |
| Required? | false |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | false |
| Accept Wildcard Characters? | false |

## Inputs

The input type is the type of the objects that you can pipe to the cmdlet.

* Sitecore.Data.Items.Item 

## Outputs

The output type is the type of the objects that the cmdlet emits.

* Sitecore.Layouts.RenderingDefinition

## Notes

Help Author: Adam Najmanowicz, Michael West

## Examples

### EXAMPLE

get all renderings for "Default" device, located in the any placeholder that has name in it or any of its sub-placeholders

```text
PS master:\> Get-Item master:\content\home | Get-Rendering -Placeholder "*main*" -Device (Get-LayoutDevice "Default")
```

## Related Topics

* [https://github.com/SitecorePowerShell/Console/](https://github.com/SitecorePowerShell/Console/) 
* [Add-Rendering](add-rendering.md)
* [New-Rendering](new-rendering.md)
* [Set-Rendering](set-rendering.md)
* [Get-LayoutDevice](get-layoutdevice.md)
* [Remove-Rendering](remove-rendering.md)
* [Get-Layout](get-layout.md)
* [Set-Layout](set-layout.md)

