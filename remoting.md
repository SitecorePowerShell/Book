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

### Authentication

{% hint style="info" %}
Introduced in SPE 9.0.
{% endhint %}

SPE remoting supports multiple authentication methods. **API keys are the recommended approach** for new integrations as they provide per-consumer security profiles, rate limiting, and user impersonation without sharing a global secret.

#### API Key Authentication (Recommended)

API keys are managed as Sitecore content items under `/sitecore/system/Modules/PowerShell/Settings/Remoting/API Keys/`. Each key can be configured with:

| Property | Description |
| :--- | :--- |
| `Shared Secret` | The authentication secret for this key. |
| `Enabled` | Activate or deactivate the key. |
| `Profile` | The restriction profile applied to sessions using this key. |
| `Impersonate User` | Optional user context for the remote session. |
| `Request Limit` | Maximum requests within the throttle window. |
| `Throttle Window` | Time window for rate limiting. |

When rate limits are exceeded, the server returns HTTP 429 with rate limit headers:

| Header | Description |
| :--- | :--- |
| `X-RateLimit-Limit` | Maximum requests allowed. |
| `X-RateLimit-Remaining` | Remaining requests in the current window. |
| `X-RateLimit-Reset` | When the rate limit resets. |

{% hint style="warning" %}
**Prefer API keys over SharedSecret.** The legacy shared secret approach uses a single global secret for all consumers. API keys provide individual secrets with per-key profiles, rate limiting, and audit trails.
{% endhint %}

#### Shared Secret Authentication (Legacy)

The original authentication method uses a single shared secret configured in `Spe.config`. This method is retained for backwards compatibility.

#### JWT Improvements

SPE 9.0 includes several JWT enhancements:

- **HS512 support** — stronger signing algorithm alongside the existing HS256
- **`iat`/`nbf` claim validation** — tokens are validated for issued-at and not-before claims
- **Configurable token lifetime** — adjust token expiration to match your security requirements
- **Proper HTTP status codes** — authentication failures now return HTTP 401 instead of 500, making it easier to distinguish auth problems from server errors

### Windows Authenticated Requests

If you have configured the web services to run under _Windows Authentication_ mode in IIS then you'll need to use the **Credential** parameter for the commands.

You'll definitely know you need it when you receive an error like the following:

```powershell
New-WebServiceProxy : The request failed with HTTP status 401: Unauthorized.
```

**Example:** The following connects Windows PowerShell ISE to a remote Sitecore instance using Windows credentials and executes the provided script.

```powershell
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

```powershell
# If you need to connect to more than one instance of Sitecore add it to the list.
$instanceUrls = @("https://remotesitecore","https://remotesitecore2")
$session = New-ScriptSession -Username admin -Password b -ConnectionUri $instanceUrls
Invoke-RemoteScript -Session $session -ScriptBlock { $env:computername }
Stop-ScriptSession -Session $session
```

### File and Media Service

We have provided a service for downloading all files and media items from the server. This disabled by default and can be enabled using a patch file. See the [Security](security/) page for more details about the services available and how to configure.

**Example:** The following downloads a single file from the _Package_ directory.

```powershell
Import-Module -Name SPE
$session = New-ScriptSession -Username admin -Password b -ConnectionUri https://remotesitecore
Receive-RemoteItem -Session $session -Path "default.js" -RootPath App -Destination "C:\Files\"
Stop-ScriptSession -Session $session
```

**Example:** The following downloads a single media item from the library.

```powershell
Import-Module -Name SPE
$session = New-ScriptSession -Username admin -Password b -ConnectionUri https://remotesitecore
Receive-RemoteItem -Session $session -Path "/Default Website/cover" -Destination "C:\Images\" -Database master
Stop-ScriptSession -Session $session
```

### Output Formats

{% hint style="info" %}
Introduced in SPE 9.0.
{% endhint %}

The `Invoke-RemoteScript` command supports three output formats through the `-OutputFormat` parameter.

| Format | Description | Use Case |
| :--- | :--- | :--- |
| `CliXml` | Default. Full type-preserving serialization via PowerShell's CliXml. | When you need deserialized PowerShell objects with type information. |
| `Json` | Structured JSON with an `{"output":[], "errors":[]}` envelope. | When performance matters or when consumers expect JSON (APIs, CI/CD). |
| `Raw` | Unstructured `.ToString()` output. | When you only need simple string output. |

JSON serialization is approximately **2.4x faster** than CliXml for large result sets while still preserving structured property data.

**Example:** The following retrieves items using JSON output format.

```powershell
Import-Module -Name SPE
$session = New-ScriptSession -Username admin -Password b -ConnectionUri https://remotesitecore
$response = Invoke-RemoteScript -Session $session -OutputFormat Json -ScriptBlock {
    Get-ChildItem -Path "master:\content\Home" | 
        Select-Object -Property Name, TemplateName, "__Updated"
}
Stop-ScriptSession -Session $session
```

The JSON response envelope separates output from errors:

```json
{
  "output": [
    { "Name": "About", "TemplateName": "Sample Item", "__Updated": "20250401T120000Z" }
  ],
  "errors": []
}
```

{% hint style="info" %}
The `-Raw` switch is equivalent to `-OutputFormat Raw` and continues to work for backwards compatibility.
{% endhint %}

### Structured Errors

{% hint style="info" %}
Introduced in SPE 9.0.
{% endhint %}

By default, errors in JSON responses are returned as flat strings. The `-StructuredErrors` switch on `Invoke-RemoteScript` opts into rich error objects that include diagnostic context for programmatic error handling.

{% hint style="warning" %}
Structured errors require `-OutputFormat Json`.
{% endhint %}

Each error object contains the following fields:

| Field | Description |
| :--- | :--- |
| `errorCategory` | The PowerShell error category (e.g., `ObjectNotFound`, `InvalidArgument`). |
| `fullyQualifiedErrorId` | Unique identifier for the error. |
| `exceptionType` | The .NET exception type name. |
| `exceptionMessage` | The exception message text. |
| `scriptStackTrace` | Stack trace showing where the error occurred in the script. |
| `invocationInfo` | Invocation details including line and column numbers. |

**Example:** The following demonstrates programmatic error handling with structured errors.

```powershell
Import-Module -Name SPE
$session = New-ScriptSession -Username admin -Password b -ConnectionUri https://remotesitecore
$response = Invoke-RemoteScript -Session $session -OutputFormat Json -StructuredErrors -ScriptBlock {
    Get-Item -Path "master:\content\NonExistent"
}
Stop-ScriptSession -Session $session

if ($response.errors) {
    foreach ($err in $response.errors) {
        Write-Warning "[$($err.errorCategory)] $($err.exceptionMessage)"
        Write-Verbose $err.scriptStackTrace
    }
}
```

A structured error response looks like the following:

```json
{
  "output": [],
  "errors": [
    {
      "errorCategory": "ObjectNotFound",
      "fullyQualifiedErrorId": "ItemNotFound,Spe.Commands.Data.GetItemCommand",
      "exceptionType": "System.Management.Automation.ItemNotFoundException",
      "exceptionMessage": "Cannot find path 'master:\\content\\NonExistent'.",
      "scriptStackTrace": "at <ScriptBlock>, <No file>: line 1",
      "invocationInfo": { "line": 1, "column": 5 }
    }
  ]
}
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

```powershell
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

```powershell
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

```powershell
Invoke-RemoteScript -ScriptBlock {
    Write-Verbose "Hello from the other side" -Verbose 4>&1
    "data"    
    Write-Verbose "Goodbye from the other side" -Verbose 4>&1
} -Session $session
```

**Example:** The following improves upon the previous example.

```powershell
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

## Restriction Profiles

{% hint style="info" %}
Introduced in SPE 9.0.
{% endhint %}

Remoting endpoints can be secured with [restriction profiles](security/restriction-profiles.md) that control language mode, command access, and content path visibility. When a profile is active, response headers indicate the restriction state:

| Header | Description |
| :--- | :--- |
| `X-SPE-Profile` | The active restriction profile name. |
| `X-SPE-LanguageMode` | `FullLanguage` or `ConstrainedLanguage`. |
| `X-SPE-Restriction` | Set on 403 responses when a command is blocked. |
| `X-SPE-BlockedCommand` | The specific command that was blocked. |

For configuration details, see:
- [Restriction Profiles](security/restriction-profiles.md) — Profile definitions and features
- [API Keys](security/api-keys.md) — Per-consumer credentials with profile binding
- [Trusted Scripts](security/trusted-scripts.md) — Allow specific scripts to bypass CLM
- [Item Path Restrictions](security/item-path-restrictions.md) — Control content tree access

## Troubleshooting

### HTTP 401 Unauthorized

Authentication failures return HTTP 401 with a descriptive message. Common causes:

- Wrong shared secret or API key
- JWT audience mismatch
- Expired or malformed token
- User not in the remoting authorization list

Check the [Logging and Monitoring](security/logging-and-monitoring.md) page for detailed auth event logs.

### HTTP 403 Forbidden (Command Blocked)

A restriction profile is blocking a command. Check the `X-SPE-BlockedCommand` response header to identify which command was blocked and `X-SPE-Profile` for the active profile. Options:

- Use an alternative command that is not blocked
- Register the script as a [trusted script](security/trusted-scripts.md)
- Adjust the profile's command list via [item-based overrides](security/restriction-profiles.md#item-based-overrides)

### HTTP 429 Too Many Requests

An [API Key](security/api-keys.md) rate limit has been exceeded. The `Retry-After` header indicates how long to wait. The `Invoke-RemoteScript` client handles this automatically.

### FileSystem Provider Error

If you receive the following error when trying to run a script (note the namespace is `Microsoft.PowerShell.Commands` instead of `Spe` or similar):

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

This issue occurs due to the fact that the remoting session defaults to the `FileSystem` provider. Changing the location activates the custom provider included with SPE. As part of the custom provider there are additional parameters added to commands native to PowerShell.

## References

* [CORS Configuration](security/web-services.md#cors-configuration) - Configure cross-origin access for SPE web services
* Michael's follow up post on [Remoting](https://michaellwest.blogspot.com/2015/07/sitecore-powershell-extensions-remoting.html)
* Adam's initial post on [Remoting](https://blog.najmanowicz.com/2014/10/10/sitecore-powershell-extensions-remoting/)

