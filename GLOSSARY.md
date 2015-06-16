# Glossary

## CLI

Command-line interface

## IIS

Internet Information Services

## ISE

Integrated Scripting Environment

## SPE

Sitecore PowerShell Extensions

# Cmdlets

## Add-ItemLanguage
[`Add-ItemLanguage`](appendix/commands/Add-ItemLanguage.html) - Cmdlet - Creates a version of the item in a new language based on an existing language version.

## New-ItemAcl
[`New-ItemAcl`](appendix/commands/New-ItemAcl.html) - Cmdlet - Creates a new access rule for use with `Set-ItemAcl` and `Add-ItemAcl` cmdlets.

## Get-ItemAcl
[`Get-ItemAcl`](appendix/commands/Get-ItemAcl.html) -Cmdlet - Retrieves security access rules from an item. Those rules can later be used with `Set-ItemAcl` and `Add-ItemAcl` cmdlets to copy them to another item or manipulated and saved back to the same item.

## Add-ItemAcl
[`Add-ItemAcl`](appendix/commands/Add-ItemAcl.html) - Cmdlet - Adds new access rule to an item allowing for the item to have the access granted or denied for a provided role or user. The new rules will be appended to the existing security descriptors on the item.

## Set-ItemAcl
[`Set-ItemAcl`](appendix/commands/Set-ItemAcl.html) - Cmdlet - Sets new security information on an item. The new rules will overwrite the existing security descriptors on the item.

## Clear-ItemAcl
[`Clear-ItemAcl`](appendix/commands/Clear-ItemAcl.html)- Cmdlet - Removes all security information from the item specified.

## Test-ItemAcl
[`Test-ItemAcl`](appendix/commands/Test-ItemAcl.html) - Cmdlet - Allows script to determine whether the specified user can perform a specified operation.
