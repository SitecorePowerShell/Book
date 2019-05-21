# Test-Rule

Tests item against a sitecore serialized rules engine rule set.

## Syntax

Test-Rule \[-Rule &lt;String&gt;\] \[-InputObject &lt;PSObject&gt;\] \[-RuleDatabase &lt;String&gt;\]

## Detailed Description

Tests item or a stream of items against a sitecore serialized rules engine rule set.

© 2010-2019 Adam Najmanowicz, Michael West. All rights reserved. Sitecore PowerShell Extensions

## Parameters

### -Rule  &lt;String&gt;

Serialized sitecore rules engine rule. Such rules can be read from rule fields or created by user with the Read-Variable cmdlet.

| Aliases |  |
| :--- | :--- |
| Required? | false |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | false |
| Accept Wildcard Characters? | false |

### -InputObject  &lt;PSObject&gt;

Item to be tested

| Aliases |  |
| :--- | :--- |
| Required? | false |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | false |
| Accept Wildcard Characters? | false |

### -RuleDatabase  &lt;String&gt;

Name of the database from which rules are pulled.

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

* System.Boolea 

## Notes

Help Author: Adam Najmanowicz, Michael West

## Examples

### EXAMPLE 1

Specifies a rule as "items that have layout" and runs the rule againste all items under the ome Item

```text
$rule = '<ruleset>
<rule uid="{9CF02118-F189-49C4-9F2B-6698D64ACF23}">
<conditions>
<condition id="{A45DBBAE-F74F-4EFE-BBD5-24395E0AF945}" uid="ED10990E15EB4E1E8FCFD33F441588A1" />
</conditions>
</rule>
</ruleset>'

Get-ChildItem master:\content\Home -Recurse | ? { Test-Rule -InputObject $_ -Rule $rule -RuleDatabase master}
```

### EXAMPLE 2

Asks user for the rule and root under which items should be filtered, and lists all items fulfilling the rule under the selected path

```text
$rule = '<ruleset></ruleset>'
$root = Get-Item master:\content\home\ 

$result = Read-Variable -Parameters `
@{Name="root"; title="Items under"; Tooltip="Items under the selected item will be considered for evaluation"}, `
@{Name="rule"; Editor="rule"; title="Filter rules"; Tooltip="Only items conforming to this rule will be displayed."} `
-Description "This dialog shows editor how a rule can be taken from an item and edited using the Read-Variable cmdlet." `
-Title "Sample rule editing" -Width 600 -Height 600 -ShowHints

if($result -eq "cancel"){
exit;
}

Get-ChildItem $root.ProviderPath | ? { Test-Rule -InputObject $_ -Rule $rule -RuleDatabase master}
```

## Related Topics

* [https://github.com/SitecorePowerShell/Console/](https://github.com/SitecorePowerShell/Console/) 

