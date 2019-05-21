# Remove-BaseTemplate

Remove one or more base templates from a template item.

## Syntax

Remove-BaseTemplate -Item &lt;Item&gt; -TemplateItem &lt;TemplateItem\[\]&gt;

Remove-BaseTemplate -Item &lt;Item&gt; -Template &lt;String\[\]&gt;

Remove-BaseTemplate -Path &lt;String&gt; -TemplateItem &lt;TemplateItem\[\]&gt;

Remove-BaseTemplate -Path &lt;String&gt; -Template &lt;String\[\]&gt;

Remove-BaseTemplate -Id &lt;String&gt; -TemplateItem &lt;TemplateItem\[\]&gt;

Remove-BaseTemplate -Id &lt;String&gt; -Template &lt;String\[\]&gt;

Remove-BaseTemplate \[-Database &lt;String&gt;\]

## Detailed Description

The Remove-BaseTemplate command removes one or more base templates from a template item.

© 2010-2019 Adam Najmanowicz, Michael West. All rights reserved. Sitecore PowerShell Extensions

## Parameters

### -Item  &lt;Item&gt;

The item to remove the base template from.

| Aliases |  |
| :--- | :--- |
| Required? | true |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | true \(ByValue\) |
| Accept Wildcard Characters? | false |

### -Path  &lt;String&gt;

Path to the item to remove the base template from.

| Aliases |  |
| :--- | :--- |
| Required? | true |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | false |
| Accept Wildcard Characters? | false |

### -Id  &lt;String&gt;

Id of the item to remove the base template from.

| Aliases |  |
| :--- | :--- |
| Required? | true |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | false |
| Accept Wildcard Characters? | false |

### -TemplateItem  &lt;TemplateItem\[\]&gt;

Sitecore item or list of items of base templates to remove.

| Aliases |  |
| :--- | :--- |
| Required? | true |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | false |
| Accept Wildcard Characters? | false |

### -Template  &lt;String\[\]&gt;

Path representing the template item to remove as a base template. This must be of the same database as the item to be altered. Note that this parameter only supports a single template.

| Aliases |  |
| :--- | :--- |
| Required? | true |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | false |
| Accept Wildcard Characters? | false |

### -Database  &lt;String&gt;

Database containing the item to remove the base template from - required if item is specified with Id.

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

## Notes

Help Author: Adam Najmanowicz, Michael West, Alex Washtell

## Examples

### EXAMPLE 1

Remove base template of /sitecore/templates/User Defined/BaseTemplate from a template, using a path.

```text
PS master:\> Remove-BaseTemplate -Path "master:/sitecore/content/User Defined/Page" -Template "/sitecore/templates/User Defined/BaseTemplate"
```

### EXAMPLE 2

Remove multiple base templates from a template, using items.

```text
PS master:\> $baseA = Get-Item -Path master:/sitecore/content/User Defined/BaseTemplateA
       PS master:\> $baseB = Get-Item -Path master:/sitecore/content/User Defined/BaseTemplateB
       PS master:\> Remove-BaseTemplate -Path "master:/sitecore/content/User Defined/Page" -TemplateItem @($baseA, $baseB)
```

## Related Topics

* [Add-BaseTemplate](add-basetemplate.md)
* [Get-ItemTemplate](get-itemtemplate.md)
* [Set-ItemTemplate](set-itemtemplate.md)
* [https://github.com/SitecorePowerShell/Console/](https://github.com/SitecorePowerShell/Console/) 

