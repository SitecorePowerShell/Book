# Remove-User

Removes the Sitecore user.

## Syntax

Remove-User \[-Identity\] &lt;AccountIdentity&gt;

Remove-User -Instance &lt;User&gt;

## Detailed Description

The Remove-User command removes a user from Sitecore.

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

### -Instance  &lt;User&gt;

| Aliases |  |
| :--- | :--- |
| Required? | true |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | true \(ByValue\) |
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
PS master:\> Remove-User -Identity michael
```

### EXAMPLE 2

```text
PS master:\> "michael","adam","mike" | Remove-User
```

### EXAMPLE 3

```text
PS master:\> Get-User -Filter sitecore\m* | Remove-User
```

## Related Topics

* [https://github.com/SitecorePowerShell/Console/](https://github.com/SitecorePowerShell/Console/) 
* [Get-User](get-user.md)
* [New-User](new-user.md)
* [Set-User](set-user.md)
* [Unlock-User](unlock-user.md)

