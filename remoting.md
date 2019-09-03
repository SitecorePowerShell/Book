# Remoting

There are a number of use cases where you need to remotely run scripts within SPE. Here we will try to cover a few of those use cases.

## Remoting Automation Service

We have provided a handy way of executing scripts via web service using the Remoting Automation Service.

### Remoting Module Setup

The setup of the module only requires a few steps: 1. In the Sitecore instance install the Sitecore module package. 2. On the local desktop or server install the SPE Remoting module.

* After downloading you may need to _unblock_ the file by right-clicking the zip and unblocking.
* Ensure that you have run `Set-ExecutionPolicy RemoteSigned` in order for the SPE Remoting module will run. This typically requires elevated privileges.
  1. Enable the _remoting_ service through a configuration patch. See the [Security](security/) page for more details.
  2. Grant the _remoting_ service user account through a configuration patch and granting acess to the appropriate role. See the [Security](security/) page for more details.

![SPE Remoting Module](https://img.youtube.com/vi/fGvT8eDdWrg/0.jpg)

[Click for a demo](https://www.youtube.com/watch?v=fGvT8eDdWrg)

The remoting services use a combination of a SOAP service \(ASMX\) and HttpHandler \(ASHX\). Remoting features are disabled by default and should be configured as needed as can be seen in the [security section here](security/). The SOAP service may require additional Windows authentication using the `-Credential` parameter which is common when logged into a Windows Active Directory domain.

### Windows Authenticated Requests

If you have configured the web services to run under _Windows Authentication_ mode in IIS then you'll need to use the **Credential** parameter for the commands.

You'll definitely know you need it when you receive an error like the following:

```text
New-WebServiceProxy : The request failed with HTTP status 401: Unauthorized.
```

**Example:** The following connects Windows PowerShell ISE to a remote Sitecore instance using Windows credentials and executes the provided script.

```text
Import-Module -Name SPE
$credential = Get-Credential
$session = New-ScriptSession -Username admin -Password b -ConnectionUri https://remotesitecore -Credential $credential
Invoke-RemoteScript -Session $session -ScriptBlock { Get-User -id admin }
Stop-ScriptSession -Session $session

# Name                     Domain       IsAdministrator IsAuthenticated
# ----                     ------       --------------- ---------------
# sitecore\admin           sitecore     True            False
```

**Example:** The following connects to several remote instances of Sitecore and returns the server name.

```text
# If you need to connect to more than one instance of Sitecore add it to the list.
$instanceUrls = @("https://remotesitecore","https://remotesitecore2")
$session = New-ScriptSession -Username admin -Password b -ConnectionUri $instanceUrls
Invoke-RemoteScript -Session $session -ScriptBlock { $env:computername }
Stop-ScriptSession -Session $session
```

### File and Media Service

We have provided a service for downloading all files and media items from the server. This disabled by default and can be enabled using a patch file. See the [Security](security/) page for more details about the services available and how to configure.

**Example:** The following downloads a single file from the _Package_ directory.

```text
Import-Module -Name SPE
$session = New-ScriptSession -Username admin -Password b -ConnectionUri https://remotesitecore
Receive-RemoteItem -Session $session -Path "default.js" -RootPath App -Destination "C:\Files\"
Stop-ScriptSession -Session $session
```

**Example:** The following downloads a single media item from the library.

```text
Import-Module -Name SPE
$session = New-ScriptSession -Username admin -Password b -ConnectionUri https://remotesitecore
Receive-RemoteItem -Session $session -Path "/Default Website/cover" -Destination "C:\Images\" -Database master
Stop-ScriptSession -Session $session
```

### Script Sessions and Web API Tutorial

![SPE Web API](https://img.youtube.com/vi/SmZBGKOryzQ/0.jpg)

[Click for a demo](https://www.youtube.com/watch?v=SmZBGKOryzQ)

## Advanced Script Sessions

Inevitably you will need to have long running processes triggered remotely. In order to support this functionality without encountering a timeout using `Invoke-RemoteScript` you can use the following list of commands.

* `Get-ScriptSession` - Returns details about script sessions.
* `Receive-ScriptSession` - Returns the results of a completed script session.
* `Remove-ScriptSession` - Removes the script session from memory.
* `Start-ScriptSession` - Executes a new script session.
* `Stop-ScriptSession` - Terminates an existing script session.
* `Wait-ScriptSession` - Waits for all the script sessions to complete before continuing.

{% hint style="info" %}
These commands are not only used for remoting, we just thought it made sense to talk about them here.
{% endhint %}

**Example:** The following remotely runs the id of a `ScriptSession` and polls the server until completed.

```text
Import-Module -Name SPE
$session = New-ScriptSession -Username admin -Password b -ConnectionUri https://remotesitecore
$jobId = Invoke-RemoteScript -Session $session -ScriptBlock {
        "master", "web" | Get-Database | 
            ForEach-Object { 
                [Sitecore.Globals]::LinkDatabase.Rebuild($_)
            }
} -AsJob
Wait-RemoteScriptSession -Session $session -Id $jobId -Delay 5 -Verbose
Stop-ScriptSession -Session $session
```

**Example:** The following remotely runs a script and checks for any output errors. The _LastErrors_ parameter is available for `ScriptSession` objects.

```text
$jobId = Invoke-RemoteScript -Session $session -ScriptBlock {
    Get-Session -ParameterDoesNotExist "SomeData"
} -AsJob
# This delay could actually be that you got up to get some coffee or tea.
Start-Sleep -Seconds 2

Invoke-RemoteScript -Session $session -ScriptBlock {
    $ss = Get-ScriptSession -Id $using:JobId
    $ss | Receive-ScriptSession

    if($ss.LastErrors) {
        $ss.LastErrors
    }
}
```

**Example:** The following redirects messages from `Write-Verbose` to the remote session. The data returned will be both `System.String` and `Deserialized.System.Management.Automation.VerboseRecord` so be sure to filter it out when needed. More information about the redirection `4>&1` can be read [here](https://github.com/SitecorePowerShell/Book/tree/a1cbd06eba0aad8913e553f4aaa08de0412c635a/[https:/blogs.technet.microsoft.com/heyscriptingguy/2014/03/30/understanding-streams-redirection-and-write-host-in-powershell/]/README.md).

```text
Invoke-RemoteScript -ScriptBlock {
    Write-Verbose "Hello from the other side" -Verbose 4>&1
    "data"    
    Write-Verbose "Goodbye from the other side" -Verbose 4>&1
} -Session $session
```

**Example:** The following improves upon the previous example.

```text
Invoke-RemoteScript -ScriptBlock {
    function Write-Verbose {
        param([string]$Message)
        Microsoft.PowerShell.Utility\Write-Verbose -Message $Message -Verbose 4>&1
    }

    Write-Verbose "Hello from the other side"
    "data"    
    Write-Verbose "Goodbye from the other side"
} -Session $session
```

## Troubleshooting

If you receive the following error when trying to run a script (note the namespace is `Microsoft.PowerShell.Commands` instead of `Cognifide.Powershell` or similar):

```
    + FullyQualifiedErrorId : NamedParameterNotFound,Microsoft.PowerShell.Commands.NewItemCommand
```

then add the following line as the first line within the Invoke-RemoteScript block: `Set-Location -Path "master:"`

**Example:**
```
Invoke-RemoteScript -ScriptBlock {
    Set-Location -Path "master:"
    ...
    [The rest of your script]
    ...
}
```

## References:

* Michael's follow up post on [Remoting](https://michaellwest.blogspot.com/2015/07/sitecore-powershell-extensions-remoting.html)
* Adam's initial post on [Remoting](https://blog.najmanowicz.com/2014/10/10/sitecore-powershell-extensions-remoting/)

