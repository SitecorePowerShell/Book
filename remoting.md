# Remoting

There are a number of use cases where you need to remotely run scripts within SPE. Here we will try to cover a few of those use cases.

#### Example: Local SPE to Remote SPE

```powershell
# Import functions
Import-Function -Name New-ScriptSession
Import-Function -Name Invoke-RemoteScript

$session = New-ScriptSession -Username "admin" -Password "b" -ConnectionUri "http://remotespe/sitecore%20modules/PowerShell/Services/RemoteAutomation.asmx"

$script1 = {
    [Sitecore.Security.Accounts.User]$user = Get-User -Identity admin
}

$script2 = {
    $user
    $params.date.ToString()
}

$args = @{
    "date" = [datetime]::Now
}

Invoke-RemoteScript -ScriptBlock $script1 -Session $session -ArgumentList $args