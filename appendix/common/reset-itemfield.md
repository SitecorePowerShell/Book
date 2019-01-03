# Reset-ItemField

Resets item fields, specified as either names, fields or template fields.

## Syntax

Reset-ItemField \[-Item\] &lt;Item&gt; \[-IncludeStandardFields\] \[-Name &lt;String\[\]&gt;\] \[-Language &lt;String\[\]&gt;\]

Reset-ItemField \[-Path\] &lt;String&gt; \[-IncludeStandardFields\] \[-Name &lt;String\[\]&gt;\] \[-Language &lt;String\[\]&gt;\]

Reset-ItemField -Id &lt;String&gt; \[-Database &lt;String&gt;\] \[-IncludeStandardFields\] \[-Name &lt;String\[\]&gt;\] \[-Language &lt;String\[\]&gt;\]

## Detailed Description

Resets item fields, specified as either names, fields or template fields.

© 2010-2019 Adam Najmanowicz, Michael West. All rights reserved. Sitecore PowerShell Extensions

## Parameters

### -IncludeStandardFields  &lt;SwitchParameter&gt;

Includes fields that are defined on "Standard template"

| Aliases |  |
| :--- | :--- |
| Required? | false |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | false |
| Accept Wildcard Characters? | false |

### -Name  &lt;String\[\]&gt;

Array of field names to include - supports wildcards.

| Aliases |  |
| :--- | :--- |
| Required? | false |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | false |
| Accept Wildcard Characters? | false |

### -Language  &lt;String\[\]&gt;

Language that will be reset. If not specified the current user language will be used. Globbing/wildcard supported.

| Aliases |  |
| :--- | :--- |
| Required? | false |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | false |
| Accept Wildcard Characters? | false |

### -Item  &lt;Item&gt;

The item to be analysed.

| Aliases |  |
| :--- | :--- |
| Required? | true |
| Position? | 1 |
| Default Value |  |
| Accept Pipeline Input? | true \(ByValue, ByPropertyName\) |
| Accept Wildcard Characters? | false |

### -Path  &lt;String&gt;

Path to the item to be analysed.

| Aliases |  |
| :--- | :--- |
| Required? | true |
| Position? | 1 |
| Default Value |  |
| Accept Pipeline Input? | false |
| Accept Wildcard Characters? | false |

### -Id  &lt;String&gt;

Id of the item to be analysed.

| Aliases |  |
| :--- | :--- |
| Required? | true |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | false |
| Accept Wildcard Characters? | false |

### -Database  &lt;String&gt;

Database containing the item to be reset - can work with Language parameter to narrow the publication scope.

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

* None 

## Notes

Help Author: Adam Najmanowicz, Michael West, Alex Washtell

## Examples

### EXAMPLE 1

Reset all item fields, excluding standard fields.

```text
PS master:\> Reset-ItemField -Path master:\content\home
```

### EXAMPLE 2

Reset all item fields, including standard fields.

```text
PS master:\> Reset-ItemField -Path master:\content\home -IncludeStandardFields
```

### EXAMPLE 3

Reset all item fields with names beginning with "a", excluding standard fields.

```text
PS master:\> Get-Item master:\content\home | Reset-ItemField -Name "a*"
```

## Related Topics

* [Get-ItemTemplate](get-itemtemplate.md)
* [Get-ItemField](get-itemfield.md)
* [https://github.com/SitecorePowerShell/Console/](https://github.com/SitecorePowerShell/Console/) 

