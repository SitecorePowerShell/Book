# Invoke-Script

Executes a script from Sitecore PowerShell Extensions Script Library. This command used to be named Execute-Script - a matching alias added for compatibility with older scripts.

## Syntax

Invoke-Script \[-Item\] &lt;Item&gt; \[-ArgumentList &lt;Object\[\]&gt;\]

Invoke-Script \[-Path\] &lt;String&gt; \[-ArgumentList &lt;Object\[\]&gt;\]

## Detailed Description

Executes a script from Sitecore PowerShell Extensions Script Library.

© 2010-2019 Adam Najmanowicz, Michael West. All rights reserved. Sitecore PowerShell Extensions\#\# Aliases

The following abbreviations are aliases for this cmdlet:

* Execute-Script 

## Parameters

### -Item  &lt;Item&gt;

The script item to be executed.

| Aliases |  |
| :--- | :--- |
| Required? | true |
| Position? | 1 |
| Default Value |  |
| Accept Pipeline Input? | true \(ByValue, ByPropertyName\) |
| Accept Wildcard Characters? | false |

### -Path  &lt;String&gt;

Path to the script item to be executed. Path can be absolute or Relavie to Script library root. e.g. the following two commands are equivalent:

PS master:\&gt; Invoke-Script 'master:\system\Modules\PowerShell\Script Library\Examples\Script Testing\Long Running Script with Progress Demo' PS master:\&gt; Invoke-Script 'Examples\Script Testing\Long Running Script with Progress Demo'

| Aliases |  |
| :--- | :--- |
| Required? | true |
| Position? | 1 |
| Default Value |  |
| Accept Pipeline Input? | false |
| Accept Wildcard Characters? | false |

### -ArgumentList  &lt;Object\[\]&gt;

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

* System.Object 

## Notes

Help Author: Adam Najmanowicz, Michael West

## Examples

### EXAMPLE

```text
PS master:\> Invoke-Script 'Examples\Script Testing\Long Running Script with Progress Demo'
```

## Related Topics

* [https://github.com/SitecorePowerShell/Console/](https://github.com/SitecorePowerShell/Console/) 
* [Import-Function](import-function.md)

