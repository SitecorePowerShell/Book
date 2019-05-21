# New-User

Creates a new Sitecore user.

## Syntax

New-User \[-Identity\] &lt;AccountIdentity&gt; \[-Password &lt;String&gt;\] \[-Email &lt;String&gt;\] \[-FullName &lt;String&gt;\] \[-Comment &lt;String&gt;\] \[-Portrait &lt;String&gt;\] \[-Enabled\] \[-ProfileItemId &lt;ID&gt;\]

## Detailed Description

The New-User command creates a new user in Sitecore.

The Identity parameter specifies the Sitecore user to create. You can specify a user by its local name or fully qualified name.

© 2010-2019 Adam Najmanowicz, Michael West. All rights reserved. Sitecore PowerShell Extensions

## Parameters

### -Identity  &lt;AccountIdentity&gt;

Specifies the Sitecore user by providing one of the following values.

```text
Local Name
    Example: developer
Fully Qualified Name
    Example: sitecore\developer
```

| Aliases |  |
| :--- | :--- |
| Required? | true |
| Position? | 1 |
| Default Value |  |
| Accept Pipeline Input? | true \(ByPropertyName\) |
| Accept Wildcard Characters? | false |

### -Password  &lt;String&gt;

| Aliases |  |
| :--- | :--- |
| Required? | false |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | true \(ByPropertyName\) |
| Accept Wildcard Characters? | false |

### -Email  &lt;String&gt;

| Aliases |  |
| :--- | :--- |
| Required? | false |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | true \(ByPropertyName\) |
| Accept Wildcard Characters? | false |

### -FullName  &lt;String&gt;

| Aliases |  |
| :--- | :--- |
| Required? | false |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | true \(ByPropertyName\) |
| Accept Wildcard Characters? | false |

### -Comment  &lt;String&gt;

| Aliases |  |
| :--- | :--- |
| Required? | false |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | true \(ByPropertyName\) |
| Accept Wildcard Characters? | false |

### -Portrait  &lt;String&gt;

| Aliases |  |
| :--- | :--- |
| Required? | false |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | true \(ByPropertyName\) |
| Accept Wildcard Characters? | false |

### -Enabled  &lt;SwitchParameter&gt;

Specifies that the account should be enabled. When enabled, the Password parameter is required.

| Aliases |  |
| :--- | :--- |
| Required? | false |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | true \(ByPropertyName\) |
| Accept Wildcard Characters? | false |

### -ProfileItemId  &lt;ID&gt;

Specifies the profile id to use for the user.

| Aliases |  |
| :--- | :--- |
| Required? | false |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | true \(ByPropertyName\) |
| Accept Wildcard Characters? | false |

## Inputs

The input type is the type of the objects that you can pipe to the cmdlet.

* System.String

  Represents the identity of a role.

## Outputs

The output type is the type of the objects that the cmdlet emits.

* None. 

## Notes

Help Author: Adam Najmanowicz, Michael West

## Examples

### EXAMPLE 1

```text
PS master:\> New-User -Identity michael
```

### EXAMPLE 2

```text
PS master:\> New-User -Identity michael -Enabled -Password b -Email michaellwest@gmail.com -FullName "Michael West"
```

### EXAMPLE 3

```text
PS master:\> New-User -Identity michael -PassThru

Name                     Domain       IsAdministrator IsAuthenticated
----                     ------       --------------- ---------------
sitecore\michael2        sitecore     False           False
```

## Related Topics

* [https://github.com/SitecorePowerShell/Console/](https://github.com/SitecorePowerShell/Console/) 
* [Get-User](get-user.md)
* [Set-User](set-user.md)
* [Remove-User](remove-user.md)
* [Unlock-User](unlock-user.md)

