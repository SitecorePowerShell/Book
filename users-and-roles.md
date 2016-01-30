# Users and Roles

Managing users and roles is a big topic and this section won't cover everything. We aim to show you different problems that have come up and how we solved them.

### Users

**Example:** The following generates a batch of test users in the default domain with the out-of-the-box user profile template. The users are then queried matching on the name.

```powershell
foreach($num in 0..10) {
    $key = -join ((65..90) + (97..122) | Get-Random -Count 7 | % {[char]$_})  
    New-User -Identity "TestUser$($key)" -Enabled -Password "b" -ProfileItemId "{AE4C4969-5B7E-4B4E-9042-B2D8701CE214}" | Out-Null
}

Get-User -Filter "sitecore\TestUser*"
```

In case you forgot to set the user profile, we have a way to update user accounts.

**Example:** The following queries a user and sets the profile template. Note that changing the profile template requires the user to be authenticated.

```powershell
Get-User -Id "michael" -Authenticated | 
    Set-User -ProfileItemId "{AE4C4969-5B7E-4B4E-9042-B2D8701CE214}"
```

**Example:** The follow queries all the user accounts for the default provider and filters those over the age of 18. The *age* property is custom on the *Profile*. We then export to CSV.

```powershell
$users = Get-User -Filter * | Where-Object { $_.Profile.GetCustomProperty("age") -gt 18 } 

$property = @(
    "Name",
    @{Name="Age";Expression={ $PSItem.Profile.GetCustomProperty("age") }}
)
$users | Select-Object -Property $property | 
  Export-CSV -Path "C:\temp\users-over-eighteen.csv" -NoTypeInformation
```

### References

* [Using Get-User command to query over 200k users][1]

[1]: http://stackoverflow.com/questions/34982451/sitecore-powershell-get-user-command/34994302#34994302
