# Users and Roles

Managing users and roles is a big topic and this section won't cover everything. We aim to show you different problems that have come up and how we solved them.

### Users

**Example:** The follow queries all the user accounts for the default provider and filters those over the age of 18. The *Age* property is custom on the *Profile*. We then export to CSV.

```powershell
$users = Get-User -Filter * | Where-Object { $_.Profile.GetCustomProperty("age") -gt 18 } 

$property = @(
    "Name",
    @{Name="Age";Expression={ $PSItem.Profile.GetCustomProperty("age") }}
)
$users | Select-Object -Property $property | 
  Export-CSV -Path c:\temp\users-over-eighteen.csv -NoTypeInformation
```

### References

* [Using Get-User command to query over 200k users][1]

[1]: http://stackoverflow.com/questions/34982451/sitecore-powershell-get-user-command/34994302#34994302
