# Set-ItemTemplate

Sets the item template.

## Syntax

Set-ItemTemplate -Item &lt;Item&gt; -TemplateItem &lt;TemplateItem&gt; \[-FieldsToCopy &lt;Hashtable&gt;\]

Set-ItemTemplate -Item &lt;Item&gt; -Template &lt;String&gt; \[-FieldsToCopy &lt;Hashtable&gt;\]

Set-ItemTemplate -Path &lt;String&gt; -TemplateItem &lt;TemplateItem&gt; \[-FieldsToCopy &lt;Hashtable&gt;\]

Set-ItemTemplate -Path &lt;String&gt; -Template &lt;String&gt; \[-FieldsToCopy &lt;Hashtable&gt;\]

Set-ItemTemplate -Id &lt;String&gt; -TemplateItem &lt;TemplateItem&gt; \[-FieldsToCopy &lt;Hashtable&gt;\]

Set-ItemTemplate -Id &lt;String&gt; -Template &lt;String&gt; \[-FieldsToCopy &lt;Hashtable&gt;\]

Set-ItemTemplate \[-Database &lt;String&gt;\] \[-FieldsToCopy &lt;Hashtable&gt;\]

## Detailed Description

The Set-ItemTemplate command sets the template for an item.

© 2010-2019 Adam Najmanowicz, Michael West. All rights reserved. Sitecore PowerShell Extensions

## Parameters

### -Item  &lt;Item&gt;

The item to set the template for.

| Aliases |  |
| :--- | :--- |
| Required? | true |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | true \(ByValue\) |
| Accept Wildcard Characters? | false |

### -Path  &lt;String&gt;

Path to the item to set the template for.

| Aliases |  |
| :--- | :--- |
| Required? | true |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | false |
| Accept Wildcard Characters? | false |

### -Id  &lt;String&gt;

Id of the item to set the template for.

| Aliases |  |
| :--- | :--- |
| Required? | true |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | false |
| Accept Wildcard Characters? | false |

### -TemplateItem  &lt;TemplateItem&gt;

Sitecore item representing the template.

| Aliases |  |
| :--- | :--- |
| Required? | true |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | false |
| Accept Wildcard Characters? | false |

### -Template  &lt;String&gt;

Path representing the template item. This must be of the same database as the item to be altered.

| Aliases |  |
| :--- | :--- |
| Required? | true |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | false |
| Accept Wildcard Characters? | false |

### -FieldsToCopy  &lt;Hashtable&gt;

Hashtable of key value pairs mapping the old template field to a new template field.

@{"Title"="Headline";"Text"="Copy"}

| Aliases |  |
| :--- | :--- |
| Required? | false |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | false |
| Accept Wildcard Characters? | false |

### -Database  &lt;String&gt;

Database containing the item to set the template for - required if item is specified with Id.

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

Set template of /sitecore/content/home item using a Template path.

```text
PS master:\> Set-ItemTemplate -Path master:/sitecore/content/home -Template "/sitecore/templates/User Defined/Page"
```

### EXAMPLE 2

Set template of /sitecore/content/home item using a TemplateItem.

```text
PS master:\> $template = Get-ItemTemplate -Path master:\content\home\page1
       PS master:\> Set-ItemTemplate -Path master:\content\home\page2 -TemplateItem $template
```

### EXAMPLE 3

Set the template and remap fields to their new name.

```text
Set-ItemTemplate -Path "master:\content\home\Page1" `
    -Template "User Defined/Target" `
    -FieldsToCopy @{Field1="Field4"; Field2="Field5"; Field3="Field6"}
```

## Related Topics

* [Get-ItemTemplate](get-itemtemplate.md)
* [Add-BaseTemplate](add-basetemplate.md)
* [Remove-BaseTemplate](remove-basetemplate.md)
* [https://github.com/SitecorePowerShell/Console/](https://github.com/SitecorePowerShell/Console/) 

