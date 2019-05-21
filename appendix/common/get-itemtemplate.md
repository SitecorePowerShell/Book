# Get-ItemTemplate

Returns the item template and its base templates.

## Syntax

Get-ItemTemplate \[-Item\] &lt;Item&gt; \[-Recurse\]

Get-ItemTemplate \[-Path\] &lt;String&gt; \[-Recurse\]

Get-ItemTemplate -Id &lt;String&gt; \[-Database &lt;String&gt;\] \[-Recurse\]

## Detailed Description

The Get-ItemTemplate command returns the item template and its base templates.

© 2010-2019 Adam Najmanowicz, Michael West. All rights reserved. Sitecore PowerShell Extensions

## Parameters

### -Recurse  &lt;SwitchParameter&gt;

Return the template the item is based on and all of its base templates.

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

Database containing the item to be analysed - required if item is specified with Id.

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

* Sitecore.Data.Items.TemplateItem 

## Notes

Help Author: Adam Najmanowicz, Michael West

## Examples

### EXAMPLE 1

Get template of /sitecore/conent/home item

```text
PS master:\> Get-ItemTemplate -Path master:\content\home

       BaseTemplates  : {Standard template}
       Fields         : {__Context Menu, __Display name, __Editor, __Editors...}
       FullName       : Sample/Sample Item
       Key            : sample item
       OwnFields      : {Title, Text, Image, State...}
       StandardValues : Sitecore.Data.Items.Item
       Database       : master
       DisplayName    : Sample Item
       Icon           : Applications/16x16/document.png
       ID             : {76036F5E-CBCE-46D1-AF0A-4143F9B557AA}
       InnerItem      : Sitecore.Data.Items.Item
       Name           : Sample Item
```

### EXAMPLE 2

Get template of /sitecore/conent/home item and all of the templates its template is based on then format it to only show the template name, path and Key

```text
PS master:\> Get-Item -Path master:/content/Home | Get-ItemTemplate -Recurse | ft Name, FullName, Key -auto

       Name              FullName                                 Key
       ----              --------                                 ---
       Sample Item       Sample/Sample Item                       sample item
       Standard template System/Templates/Standard template       standard template
       Advanced          System/Templates/Sections/Advanced       advanced
       Appearance        System/Templates/Sections/Appearance     appearance
       Help              System/Templates/Sections/Help           help
       Layout            System/Templates/Sections/Layout         layout
       Lifetime          System/Templates/Sections/Lifetime       lifetime
       Insert Options    System/Templates/Sections/Insert Options insert options
       Publishing        System/Templates/Sections/Publishing     publishing
       Security          System/Templates/Sections/Security       security
       Statistics        System/Templates/Sections/Statistics     statistics
       Tasks             System/Templates/Sections/Tasks          tasks
       Validators        System/Templates/Sections/Validators     validators
       Workflow          System/Templates/Sections/Workflow       workflow
```

## Related Topics

* [Get-ItemField](get-itemfield.md)
* [Set-ItemTemplate](set-itemtemplate.md)
* [Add-BaseTemplate](add-basetemplate.md)
* [Remove-BaseTemplate](remove-basetemplate.md)
* [https://github.com/SitecorePowerShell/Console/](https://github.com/SitecorePowerShell/Console/) 

