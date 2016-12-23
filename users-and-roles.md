# Users and Roles

Managing users and roles is a big topic and this section won't cover everything. We aim to show you different problems that have come up and how we solved them.

**Example:** The following command returns the security commands available.
```powershell
Get-Command -Noun Role*,User,ItemAcl* | Sort-Object -Property Noun,Verb
```

### Users

Managing users should be a pretty straight forward task. While the User Manager provided by Sitecore is handy, you'll likely find yourself wanting to make bulk changes. The following examples should give you a few ideas about how to manage user accounts.

#### Query users and update properties

**Example:** The following generates a batch of test users in the default domain with the out-of-the-box user profile template. The users are then queried filtering on the name.

```powershell
foreach($num in 0..10) {
    $key = -join ((65..90) + (97..122) | Get-Random -Count 7 | % {[char]$_})  
    New-User -Identity "TestUser$($key)" -Enabled -Password "b" -ProfileItemId "{AE4C4969-5B7E-4B4E-9042-B2D8701CE214}" | Out-Null
}

Get-User -Filter "sitecore\TestUser*"
```

In case you forgot to set the user profile for accounts, we have a solution for that.

**Example:** The following queries a user and sets the profile template. Note that changing the profile template requires the user to be authenticated.

```powershell
Get-User -Id "michael" -Authenticated | 
    Set-User -ProfileItemId "{AE4C4969-5B7E-4B4E-9042-B2D8701CE214}"
```

**Example:** The follow queries all the user accounts for the default provider and filters those over the age of 18. The *age* property is custom on the *Profile*. Finally, export to CSV.

```powershell
$users = Get-User -Filter * | Where-Object { $_.Profile.GetCustomProperty("age") -gt 18 } 

$property = @(
    "Name",
    @{Name="Age";Expression={ $PSItem.Profile.GetCustomProperty("age") }}
)
$users | Select-Object -Property $property | 
  Export-CSV -Path "C:\temp\users-over-eighteen.csv" -NoTypeInformation
```

##### Active Directory 

When using the [Active Directory module][2] you may need to increase the setting `LDAP.SizeLimit` if you wish to return all Active Directory accounts. 

When using `Set-User` to update AD accounts you'll likely receive an access denied message which is due to the fact that the account querying users does not have write access to profile properties.

### Roles

**Example:** The following queries roles using the specified identity.
```powershell
# Identity can be "[domain]\[name]", "Creator-Owner", and "\Everyone"
Get-Role -Identity "default\Everyone"
```

### Item Access Control Lists (ACL)

The ACL commands provide an automated way of granting privileges to items.

**Example:** The following creates a new ACL and assigns to an item.
```powershell
$aclForEveryone = New-ItemAcl -Identity "\Everyone" -PropagationType Any -SecurityPermission DenyInheritance -AccessRight *
Get-Item -Path "master:\content\home" | Add-ItemAcl -AccessRules $aclForEveryone -PassThru
```

### References

* [Using Get-User command to query over 200k users][1]

[1]: http://stackoverflow.com/questions/34982451/sitecore-powershell-get-user-command/34994302#34994302
[2]: https://dev.sitecore.net/Downloads/Active_Directory/
