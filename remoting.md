# Remoting

There are a number of use cases where you need to remotely run scripts within SPE. Here we will try to cover a few of those use cases.

#### Communication between Sitecore instances

**Example:** The following connects a local instance of SPE to a remote instance and executes the provided script.

```powershell
Import-Function -Name New-ScriptSession
Import-Function -Name Invoke-RemoteScript

$url = "http://remotespe/sitecore%20modules/PowerShell/Services/RemoteAutomation.asmx"
$session = New-ScriptSession -Username "admin" -Password "b" -ConnectionUri $url

$script = {
    [Sitecore.Security.Accounts.User]$user = Get-User -Identity admin
    $user
    $params.date.ToString()
}

$args = @{
    "date" = [datetime]::Now
}

Invoke-RemoteScript -ScriptBlock $script -Session $session -ArgumentList $args

Name                     Domain       IsAdministrator IsAuthenticated
----                     ------       --------------- ---------------
sitecore\admin           sitecore     True            False          
4/26/2015 6:15:41 PM

```