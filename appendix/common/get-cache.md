# Get-Cache

Retrieves a Sitecore cache.

## Syntax

Get-Cache \[\[-Name\] &lt;String&gt;\]

## Detailed Description

The Get-Cache command retrieves a Sitecore cache.

© 2010-2019 Adam Najmanowicz, Michael West. All rights reserved. Sitecore PowerShell Extensions

## Parameters

### -Name  &lt;String&gt;

Name of the cache to retrieve. Supports wildcards.

| Aliases |  |
| :--- | :--- |
| Required? | false |
| Position? | 1 |
| Default Value |  |
| Accept Pipeline Input? | false |
| Accept Wildcard Characters? | false |

## Outputs

The output type is the type of the objects that the cmdlet emits.

* Sitecore.Caching.Cache 

## Notes

Help Author: Adam Najmanowicz, Michael West

## Examples

### EXAMPLE

```text
PS master:\> Get-Cache -Name master*

Name                                     Enabled       Count       Size   Max Size Default  Scavengable
                                                                                   Priority
----                                     -------       -----       ----   -------- -------- -----------
master[blobIDs]                          True              0          0     512000   Normal       False
master[blobIDs]                          True              0          0     512000   Normal       False
master[blobIDs]                          True              0          0     512000   Normal       False
master[itempaths]                        True            292     108228   10485760   Normal       False
master[standardValues]                   True             57      38610     512000   Normal       False
master[paths]                            True            108      13608     512000   Normal       False
master[items]                            True           1010    5080300   10485760   Normal       False
master[data]                             True           3427    7420654   20971520   Normal       False
```

## Related Topics

* [https://github.com/SitecorePowerShell/Console/](https://github.com/SitecorePowerShell/Console/) 

