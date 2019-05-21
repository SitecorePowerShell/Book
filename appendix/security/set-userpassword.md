# Set-UserPassword

Sets the Sitecore user password.

## Syntax

Set-UserPassword \[-Identity\] &lt;AccountIdentity&gt; -OldPassword &lt;String&gt; \[-NewPassword &lt;String&gt;\]

Set-UserPassword \[-Identity\] &lt;AccountIdentity&gt; -Reset \[-NewPassword &lt;String&gt;\]

## Detailed Description

The Set-UserPassword command resets or changes a user password.

The Identity parameter specifies the Sitecore user to remove. You can specify a user by its local name or fully qualified name.

© 2010-2019 Adam Najmanowicz, Michael West. All rights reserved. Sitecore PowerShell Extensions

## Parameters

### -Identity  &lt;AccountIdentity&gt;

Specifies the Sitecore user by providing one of the following values.

```text
Local Name
    Example: admin
Fully Qualified Name
    Example: sitecore\admi
```

| Aliases |  |
| :--- | :--- |
| Required? | true |
| Position? | 1 |
| Default Value |  |
| Accept Pipeline Input? | true \(ByValue\) |
| Accept Wildcard Characters? | false |

### -NewPassword  &lt;String&gt;

| Aliases |  |
| :--- | :--- |
| Required? | false |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | true \(ByPropertyName\) |
| Accept Wildcard Characters? | false |

### -OldPassword  &lt;String&gt;

| Aliases |  |
| :--- | :--- |
| Required? | true |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | false |
| Accept Wildcard Characters? | false |

### -Reset  &lt;SwitchParameter&gt;

| Aliases |  |
| :--- | :--- |
| Required? | true |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | false |
| Accept Wildcard Characters? | false |

## Inputs

The input type is the type of the objects that you can pipe to the cmdlet.

* System.String

  Represents the identity of a user.

Sitecore.Security.Accounts.User Represents the instance of a user.

## Outputs

The output type is the type of the objects that the cmdlet emits.

* None. 

## Notes

Help Author: Adam Najmanowicz, Michael West

## Examples

### EXAMPLE 1

```text
PS master:\> Set-UserPassword -Identity michael -NewPassword pass123 -OldPassword b
```

### EXAMPLE 2

```text
PS master:\> "michael","adam","mike" | Set-UserPassword -NewPassword b -Reset
```

## Related Topics

* [https://github.com/SitecorePowerShell/Console/](https://github.com/SitecorePowerShell/Console/) 
* [Get-User](get-user.md)
* [Set-User](set-user.md)

