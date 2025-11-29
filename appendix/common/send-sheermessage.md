# Send-SheerMessage

Sends a sheer message to the app in which context the script is executing.

## Syntax

```powershell
Send-SheerMessage [-Name] <String> [-Parameters <Hashtable>] [-OnScriptEnd <SwitchParameter>] [<CommonParameters>]

Send-SheerMessage [-Name] <String> [-Parameters <Hashtable>] [<CommonParameters>]

Send-SheerMessage [-Name] <String> [-GetResult <SwitchParameter>] [-Parameters <Hashtable>] [<CommonParameters>]
```

## Detailed Description

Sends a sheer message to the app in which context the script is executing.

© 2010-2019 Adam Najmanowicz, Michael West. All rights reserved. Sitecore PowerShell Extensions

## Parameters

### -Name &lt;String&gt;

Name of the Sheer message to execute.

| Aliases                     |                  |
| :-------------------------- | :--------------- |
| Required?                   | true             |
| Position?                   | 1                |
| Default Value               |                  |
| Accept Pipeline Input?      | true \(ByValue\) |
| Accept Wildcard Characters? | false            |

### -GetResult &lt;SwitchParameter&gt;

| Aliases                     |       |
| :-------------------------- | :---- |
| Required?                   | false |
| Position?                   | named |
| Default Value               |       |
| Accept Pipeline Input?      | false |
| Accept Wildcard Characters? | false |

### -Parameters &lt;Hashtable&gt;

| Aliases                     |       |
| :-------------------------- | :---- |
| Required?                   | false |
| Position?                   | named |
| Default Value               |       |
| Accept Pipeline Input?      | false |
| Accept Wildcard Characters? | false |

### -OnScriptEnd &lt;SwitchParameter&gt;

| Aliases                     |       |
| :-------------------------- | :---- |
| Required?                   | false |
| Position?                   | named |
| Default Value               |       |
| Accept Pipeline Input?      | false |
| Accept Wildcard Characters? | false |

## Notes

Help Author: Adam Najmanowicz, Michael West

## Examples

### EXAMPLE

If you execute the following script in the PowerShell ISE the Save dialog will popup

```powershell
Send-SheerMessage -Name 'ise:save'
```

### EXAMPLE

```powershell
$item = $SitecoreContextItem
Send-SheerMessage -OnScriptEnd -Name "item:refresh" -Parameters @{id=&{$item.ID};language=&{$item.Language};version=&{$item.Version}}
Invoke-JavaScript -OnScriptEnd -Script "scForm.invoke('item:refresh(id=$($item.ID),language=$($item.Language),version=$($item.Version))')"

Send-SheerMessage -OnScriptEnd -Name "item:refreshchildren" -Parameters @{id=&{$item.Parent.ID}}
Invoke-JavaScript -OnScriptEnd -Script "scForm.invoke('item:refreshchildren(id=$($item.Parent.ID))')"

Send-SheerMessage -OnScriptEnd -Name "item:refreshchildren" -Parameters @{id=&{$item.ID};language=&{$item.Language};version=&{$item.Version}}
Invoke-JavaScript -OnScriptEnd -Script "scForm.invoke('item:refreshchildren(id=$($item.ID),language=$($item.Language),version=$($item.Version))')"

Close-Window
```

## Related Topics

- [#149](https://github.com/SitecorePowerShell/Console/issues/149)
- [#1340](https://github.com/SitecorePowerShell/Console/issues/1340)
