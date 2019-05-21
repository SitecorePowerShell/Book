# Set-HostProperty

Sets the current host property.

## Syntax

Set-HostProperty \[-ForegroundColor &lt;Black \| DarkBlue \| DarkGreen \| DarkCyan \| DarkRed \| DarkMagenta \| DarkYellow \| Gray \| DarkGray \| Blue \| Green \| Cyan \| Red \| Magenta \| Yellow \| White&gt;\] \[-BackgroundColor &lt;Black \| DarkBlue \| DarkGreen \| DarkCyan \| DarkRed \| DarkMagenta \| DarkYellow \| Gray \| DarkGray \| Blue \| Green \| Cyan \| Red \| Magenta \| Yellow \| White&gt;\] \[-HostWidth &lt;Int32&gt;\] \[-Persist\]

## Detailed Description

Sets the current host property and perssits them for the future if used with -Persist parameter.

© 2010-2019 Adam Najmanowicz, Michael West. All rights reserved. Sitecore PowerShell Extensions

## Parameters

### -ForegroundColor  &lt;ConsoleColor&gt;

Color of the console text.

| Aliases |  |
| :--- | :--- |
| Required? | false |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | false |
| Accept Wildcard Characters? | false |

### -BackgroundColor  &lt;ConsoleColor&gt;

Color of the console background.

| Aliases |  |
| :--- | :--- |
| Required? | false |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | false |
| Accept Wildcard Characters? | false |

### -HostWidth  &lt;Int32&gt;

Width of the text buffer \(texts longer than the number provided will wrap to the next line.

| Aliases |  |
| :--- | :--- |
| Required? | false |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | false |
| Accept Wildcard Characters? | false |

### -Persist  &lt;SwitchParameter&gt;

Persist the console setting provided

| Aliases |  |
| :--- | :--- |
| Required? | false |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | false |
| Accept Wildcard Characters? | false |

## Notes

Help Author: Adam Najmanowicz, Michael West

## Examples

### EXAMPLE 1

Set width of the console buffer to 80 and persist it for the future instances

```text
PS master:\> Set-HostProperty -HostWidth 80 -Persist
```

### EXAMPLE 2

Set color of the console text to cyan. Next instance of the console will revert to default \(white\).

```text
PS master:\> Set-HostProperty -ForegroundColor Cyan
```

## Related Topics

* [https://github.com/SitecorePowerShell/Console/](https://github.com/SitecorePowerShell/Console/) 

