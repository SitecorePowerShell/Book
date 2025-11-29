# Show-ModalDialog

Shows Sitecore Sheer control as a modal dialog.

## Syntax

```powershell
Show-ModalDialog -Control <String> [-Parameters <Hashtable>] [-HandleParameters <Hashtable>] [-Title <String>] [-Icon <String>] [-Width <Int32>] [-Height <Int32>] [<CommonParameters>]

Show-ModalDialog -Url <String> [-HandleParameters <Hashtable>] [-Title <String>] [-Icon <String>] [-Width <Int32>] [-Height <Int32>] [<CommonParameters>]
```

## Detailed Description

Shows Sitecore Sheer control as a modal dialog. If control returns a value - the value will be passed back as the result of the command execution.

© 2010-2019 Adam Najmanowicz, Michael West. All rights reserved. Sitecore PowerShell Extensions

## Parameters

### -Control &lt;String&gt;

Name of the Sitecore Sheer control to show

| Aliases                     |       |
| :-------------------------- | :---- |
| Required?                   | true  |
| Position?                   | named |
| Default Value               |       |
| Accept Pipeline Input?      | false |
| Accept Wildcard Characters? | false |

### -Url &lt;String&gt;

A fully formed URL that constitutes a control execution request.

| Aliases                     |       |
| :-------------------------- | :---- |
| Required?                   | true  |
| Position?                   | named |
| Default Value               |       |
| Accept Pipeline Input?      | false |
| Accept Wildcard Characters? | false |

### -Parameters &lt;Hashtable&gt;

Hashtable of parameters to pass to the control in the url.

| Aliases                     |       |
| :-------------------------- | :---- |
| Required?                   | false |
| Position?                   | named |
| Default Value               |       |
| Accept Pipeline Input?      | false |
| Accept Wildcard Characters? | false |

### -HandleParameters &lt;Hashtable&gt;

| Aliases                     |       |
| :-------------------------- | :---- |
| Required?                   | false |
| Position?                   | named |
| Default Value               |       |
| Accept Pipeline Input?      | false |
| Accept Wildcard Characters? | false |

### -Title &lt;String&gt;

Title of the control dialog.

| Aliases                     |       |
| :-------------------------- | :---- |
| Required?                   | false |
| Position?                   | named |
| Default Value               |       |
| Accept Pipeline Input?      | false |
| Accept Wildcard Characters? | false |

### -Width &lt;Int32&gt;

Width of the control dialog.

| Aliases                     |       |
| :-------------------------- | :---- |
| Required?                   | false |
| Position?                   | named |
| Default Value               |       |
| Accept Pipeline Input?      | false |
| Accept Wildcard Characters? | false |

### -Height &lt;Int32&gt;

Height of the control dialog.

| Aliases                     |       |
| :-------------------------- | :---- |
| Required?                   | false |
| Position?                   | named |
| Default Value               |       |
| Accept Pipeline Input?      | false |
| Accept Wildcard Characters? | false |

## Inputs

The input type is the type of the objects that you can pipe to the cmdlet.

- Sitecore.Data.Items.Item

## Outputs

The output type is the type of the objects that the cmdlet emits.

- System.String

## Notes

Help Author: Adam Najmanowicz, Michael West

## Examples

### EXAMPLE

```powershell
PS master:\> Show-ModalDialog -Control "ConfirmChoice" -Parameters @{btn_0="Yes (returns btn_0)"; btn_1="No (returns btn_1)"; btn_2="return btn_2"; te="Message Text"; cp="My Caption"} -Height 120 -Width 400
```

### EXAMPLE

```powershell
Show-ModalDialog -Control "PowerShellRunner" `
    -Parameter @{
        id="{110D559F-DEA5-42EA-9C1C-8A5DF7E70EF9}";
        db="master";
        lang="en";
        ver="1"
        scriptId="{16670A5E-AA62-4EE2-A9C3-C8466BEB45C2}"
        scriptDb="master"
    }
```

## Related Topics

- [Read-Variable](read-variable.md)
- [Show-Alert](show-alert.md)
- [Show-Application](show-application.md)
- [Show-Confirm](show-confirm.md)
- [Show-FieldEditor](show-fieldeditor.md)
- [Show-Input](show-input.md)
- [Show-ListView](show-listview.md)
- [Show-Result](show-result.md)
- [Show-YesNoCancel](show-yesnocancel.md)
- [#1336](https://github.com/SitecorePowerShell/Console/issues/1336)
