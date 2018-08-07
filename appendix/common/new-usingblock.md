# New-UsingBlock

New-UsingBlock.

## Syntax

New-UsingBlock \[-InputObject\] &lt;IDisposable&gt; \[-ScriptBlock\] &lt;ScriptBlock&gt;

## Detailed Description

New-UsingBlock.

© 2010-2017 Adam Najmanowicz, Michael West. All rights reserved. Sitecore PowerShell Extensions

## Parameters

### -InputObject  &lt;IDisposable&gt;

Object that should be disposed after the Script block is executed.

| Aliases |  |
| :--- | :--- |
| Required? | true |
| Position? | 1 |
| Default Value |  |
| Accept Pipeline Input? | false |
| Accept Wildcard Characters? | false |

### -ScriptBlock  &lt;ScriptBlock&gt;

Script to be executed within the "Using" context.

| Aliases |  |
| :--- | :--- |
| Required? | true |
| Position? | 2 |
| Default Value |  |
| Accept Pipeline Input? | false |
| Accept Wildcard Characters? | false |

## Inputs

The input type is the type of the objects that you can pipe to the cmdlet.

* System.IDisposable 

## Outputs

The output type is the type of the objects that the cmdlet emits.

* void 

## Notes

Help Author: Adam Najmanowicz, Michael West

## Examples

### EXAMPLE 1

Assuming all items under /sitecore/content/home have both 'Title' and 'MetaTitle' fields... Using New-UsingBlock to bulk update items under /sitecore/Content/ to have their 'MetaTitle' field to be equal to the 'Title' field

```text
New-UsingBlock (New-Object Sitecore.Data.BulkUpdateContext) {
foreach ( $item in (Get-ChildItem -Path master:\Content\Home -Recurse -WithParent) ) {
        $item."MetaTitle" = $item.Title
    }
}
```

### EXAMPLE 2

Using New-UsingBlock to perform a test with UserSwitcher - checking whether an anonymous user can change a field The test should end up showing the error as below and the Title should not be changed!

```text
$anonymous = Get-User -Identity "extranet\Anonymous"
$testItem = Get-Item -Path master:\Content\Home

New-UsingBlock (New-Object Sitecore.Security.Accounts.UserSwitcher $anonymous) {
    $testItem.Title = "If you can see this title it means that anonymous users can change this item!"
}


New-UsingBlock : Exception setting "Title": "Exception calling "Modify" with "3" argument(s): "The current user does not have write access to this item. User: extranet\Anonymous, Item: Home ({110D559F-DEA5-42EA-9C1C-8A5DF7E70EF9})""
At line:3 char:1
+ New-UsingBlock (New-Object Sitecore.Security.Accounts.UserSwitcher $a ...
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : NotSpecified: (:) [New-UsingBlock], SetValueInvocationException
    + FullyQualifiedErrorId : ScriptSetValueRuntimeException,Cognifide.PowerShell.Commandlets.Data.NewUsingBlockCommand
```

## Related Topics

* [https://github.com/SitecorePowerShell/Console/](https://github.com/SitecorePowerShell/Console/) 

