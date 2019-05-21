# Disable-User

Disables the specified Sitecore user.

## Syntax

Disable-User \[-Identity\] &lt;AccountIdentity&gt;

Disable-User -Instance &lt;User&gt;

## Detailed Description

The Disable-User command gets a user and disables the account in Sitecore.

The Identity parameter specifies the Sitecore user to get. You can specify a user by its local name or fully qualified name. You can also specify user object variable, such as $&lt;user&gt;.

© 2010-2019 Adam Najmanowicz, Michael West. All rights reserved. Sitecore PowerShell Extensions

## Parameters

### -Identity  &lt;AccountIdentity&gt;

Specifies the Sitecore user by providing one of the following values.

```text
Local Name
    Example: michael
Fully Qualified Name
    Example: sitecore\michael
```

| Aliases |  |
| :--- | :--- |
| Required? | true |
| Position? | 1 |
| Default Value |  |
| Accept Pipeline Input? | true \(ByValue\) |
| Accept Wildcard Characters? | false |

### -Instance  &lt;User&gt;

Specifies the Sitecore user by providing an instance of a user.

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

Sitecore.Security.Accounts.User One or more user instances.

## Outputs

The output type is the type of the objects that the cmdlet emits.

* None. 

## Notes

Michael West

## Examples

### EXAMPLE 1

```text
PS master:\> Disable-User -Identity michael
```

### EXAMPLE 2

```text
PS master:\> Get-User -Filter * | Disable-User
```

## Related Topics

* [https://michaellwest.blogspot.com](https://michaellwest.blogspot.com) 

