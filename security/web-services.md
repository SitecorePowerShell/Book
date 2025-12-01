# Web Services Security

SPE provides several web services for external access and automation. By default, most services are **disabled** to minimize the attack surface. This guide explains each service and how to secure them when enabled.

## Overview

Web services enable powerful capabilities like:
- Remote script execution (SPE Remoting)
- RESTful API access
- File and media uploads/downloads
- Report exports

{% hint style="danger" %}
**Security Warning:** Only enable web services you specifically need. Each enabled service increases your attack surface. Never enable services on internet-facing servers.
{% endhint %}

## Service Configuration

Services are configured in `App_Config\Include\Spe\Spe.config`:

```xml
<sitecore>
  <powershell>
    <services>
      <restfulv1 enabled="false" />
      <restfulv2 enabled="false" />
      <remoting enabled="false" />
      <fileDownload enabled="false" />
      <fileUpload enabled="false" />
      <mediaDownload enabled="false" />
      <mediaUpload enabled="false" />
      <handleDownload enabled="true" />
      <client enabled="true" />
      <execution enabled="true" />
    </services>
  </powershell>
</sitecore>
```

## Service Descriptions

### Client Service (enabled by default)

**Service File:** `PowerShellWebService.asmx`

**Purpose:** Powers the SPE Console and ISE user interfaces.

**Required For:**
- PowerShell Console
- PowerShell ISE (Integrated Scripting Environment)

**Security Considerations:**
- Required for basic SPE functionality
- Access controlled by Sitecore user permissions
- Protected by Session Elevation (UAC)

**Should you disable it?** No - this breaks the Console and ISE.

### Execution Service (enabled by default)

**Service File:** Various pipeline integrations

**Purpose:** Checks if users have permission to run SPE applications.

**Required For:**
- Download File dialogs
- PowerShell Script Runner
- Content Editor integrations (Context Menu, Insert Options, Ribbon)

**Security Considerations:**
- Authorization check service
- Does not execute arbitrary code
- Validates permissions before allowing access

**Should you disable it?** No - this breaks SPE integration points.

### Handle Download Service (enabled by default)

**Service File:** `RemoteScriptCall.ashx`

**Purpose:** Enables file downloads through the Sitecore interface.

**Required For:**
- `Out-Download` command
- Report exports (CSV, Excel, etc.)
- ISE script exports

**Security Considerations:**
- Only works for authenticated users
- Requires active Sitecore session
- Downloads are temporary and time-limited

**Should you disable it?** Only if you never need to download files from SPE (rare).

### Remoting Service (disabled by default)

**Service File:** `RemoteAutomation.asmx`

**Purpose:** Allows external clients to execute PowerShell scripts remotely.

**Required For:**
- SPE Remoting module
- Automated CI/CD scripts
- External automation tools

**Security Considerations:**
- ⚠️ HIGH RISK - enables remote code execution
- Must be protected with role-based authorization
- Should require HTTPS
- Ideal for CI/CD environments with proper security

**Example Use Case:** Automated content deployment from build servers.

#### Securing Remoting Service

**Enable with HTTPS requirement:**

```xml
<configuration xmlns:patch="https://www.sitecore.net/xmlconfig/">
  <sitecore>
    <powershell>
      <services>
        <remoting>
          <patch:attribute name="enabled">true</patch:attribute>
          <patch:attribute name="requireSecureConnection">true</patch:attribute>
          <authorization>
            <add Permission="Allow" IdentityType="Role" Identity="sitecore\PowerShell Extensions Remoting" />
          </authorization>
        </remoting>
      </services>
    </powershell>
  </sitecore>
</configuration>
```

**Restrict to specific users:**

```xml
<remoting>
  <patch:attribute name="enabled">true</patch:attribute>
  <patch:attribute name="requireSecureConnection">true</patch:attribute>
  <authorization>
    <add Permission="Allow" IdentityType="User" Identity="sitecore\automation-user" />
  </authorization>
</remoting>
```

{% hint style="warning" %}
**Load Balancer Note:** When using `requireSecureConnection` behind a load balancer that handles TLS termination, you may receive 403 errors. The backend server receives HTTP traffic and .NET doesn't recognize it as secure. Consider network-level security instead.
{% endhint %}

### RESTful v2 Service (disabled by default)

**Service File:** `RemoteScriptCall.ashx`

**Purpose:** Execute scripts via RESTful URLs with all parameters in the URL.

**Required For:**
- PowerShell Web API
- RESTful script endpoints
- External integrations

**Security Considerations:**
- ⚠️ MEDIUM-HIGH RISK - exposes script execution via HTTP
- URL-based parameters may be logged
- Should require HTTPS
- Should use POST instead of GET when possible

**Example Use Case:** Providing a web API for external systems to query Sitecore content.

#### Enabling RESTful v2

```xml
<configuration xmlns:patch="https://www.sitecore.net/xmlconfig/">
  <sitecore>
    <powershell>
      <services>
        <restfulv2>
          <patch:attribute name="enabled">true</patch:attribute>
        </restfulv2>
      </services>
    </powershell>
  </sitecore>
</configuration>
```

### File Download Service (disabled by default)

**Service File:** `RemoteScriptCall.ashx`

**Purpose:** Download files from the server file system via URL.

**Required For:**
- SPE Remoting file downloads
- External file retrieval

**Security Considerations:**
- ⚠️ MEDIUM RISK - exposes file system
- Restricted by file type and location configuration
- Only works with allowed paths

**Should you enable it?** Only if using SPE Remoting and need file downloads.

### File Upload Service (disabled by default)

**Service File:** `RemoteScriptCall.ashx`

**Purpose:** Upload files to the server file system via URL.

**Required For:**
- SPE Remoting file uploads
- External file deployment

**Security Considerations:**
- ⚠️ HIGH RISK - allows writing to file system
- Restricted by file type and location configuration
- Can be used to deploy malicious files if misconfigured
- Must be carefully controlled

**Should you enable it?** Only if absolutely necessary, with strict restrictions.

#### Securing File Upload

See [File Upload Restrictions](file-upload-restrictions.md) for detailed configuration.

### Media Download Service (disabled by default)

**Service File:** `RemoteScriptCall.ashx`

**Purpose:** Download media library items via URL.

**Required For:**
- SPE Remoting media downloads
- External media retrieval

**Security Considerations:**
- ⚠️ LOW-MEDIUM RISK - exposes media library
- Respects Sitecore security
- May expose sensitive media items

**Should you enable it?** Only if using SPE Remoting and need media downloads.

### Media Upload Service (disabled by default)

**Service File:** `RemoteScriptCall.ashx`

**Purpose:** Upload files as media library items via URL.

**Required For:**
- SPE Remoting media uploads
- External media deployment

**Security Considerations:**
- ⚠️ MEDIUM RISK - allows media library uploads
- Restricted by file type configuration
- Can bloat media library if misused

**Should you enable it?** Only if using SPE Remoting and need media uploads.

### RESTful v1 Service (disabled by default - DEPRECATED)

**Service File:** `RemoteScriptCall.ashx`

**Purpose:** Legacy RESTful API from early SPE versions.

**Security Considerations:**
- ⚠️ DEPRECATED - avoid using
- Use RESTful v2 instead

**Should you enable it?** No - migrate to v2.

## Configuration Examples

### Minimal Configuration (UI Only)

Default configuration - only services needed for Console and ISE:

```xml
<services>
  <restfulv1 enabled="false" />
  <restfulv2 enabled="false" />
  <remoting enabled="false" />
  <fileDownload enabled="false" />
  <fileUpload enabled="false" />
  <mediaDownload enabled="false" />
  <mediaUpload enabled="false" />
  <handleDownload enabled="true" />
  <client enabled="true" />
  <execution enabled="true" />
</services>
```

### CI/CD Configuration

Enable remoting for build automation, with strict security:

```xml
<configuration xmlns:patch="https://www.sitecore.net/xmlconfig/">
  <sitecore>
    <powershell>
      <services>
        <remoting>
          <patch:attribute name="enabled">true</patch:attribute>
          <patch:attribute name="requireSecureConnection">true</patch:attribute>
          <authorization>
            <patch:delete />
          </authorization>
          <authorization>
            <add Permission="Allow" IdentityType="User" Identity="sitecore\build-automation" />
          </authorization>
        </remoting>
        <fileDownload>
          <patch:attribute name="enabled">true</patch:attribute>
        </fileDownload>
      </services>
    </powershell>
  </sitecore>
</configuration>
```

### Web API Configuration

Enable RESTful v2 for web API endpoints:

```xml
<configuration xmlns:patch="https://www.sitecore.net/xmlconfig/">
  <sitecore>
    <powershell>
      <services>
        <restfulv2>
          <patch:attribute name="enabled">true</patch:attribute>
        </restfulv2>
      </services>
    </powershell>
  </sitecore>
</configuration>
```

### Full SPE Remoting

Enable all remoting capabilities (use with caution):

```xml
<configuration xmlns:patch="https://www.sitecore.net/xmlconfig/">
  <sitecore>
    <powershell>
      <services>
        <remoting>
          <patch:attribute name="enabled">true</patch:attribute>
          <patch:attribute name="requireSecureConnection">true</patch:attribute>
        </remoting>
        <fileDownload>
          <patch:attribute name="enabled">true</patch:attribute>
        </fileDownload>
        <fileUpload>
          <patch:attribute name="enabled">true</patch:attribute>
        </fileUpload>
        <mediaDownload>
          <patch:attribute name="enabled">true</patch:attribute>
        </mediaDownload>
        <mediaUpload>
          <patch:attribute name="enabled">true</patch:attribute>
        </mediaUpload>
      </services>
    </powershell>
  </sitecore>
</configuration>
```

## Role-Based Authorization

Restrict remoting access to specific roles or users.

### Default Authorization

By default, these roles can use remoting (when enabled):

```xml
<authorization>
  <add Permission="Allow" IdentityType="Role" Identity="sitecore\PowerShell Extensions Remoting" />
</authorization>
```

### Replace Default Roles

Remove default roles and add your own:

```xml
<authorization>
  <patch:delete />
</authorization>
<authorization>
  <add Permission="Allow" IdentityType="Role" Identity="sitecore\Automation" />
  <add Permission="Allow" IdentityType="Role" Identity="sitecore\Developers" />
</authorization>
```

### Add Specific Users

Grant access to individual users:

```xml
<authorization>
  <add Permission="Allow" IdentityType="User" Identity="sitecore\ci-user" />
  <add Permission="Allow" IdentityType="User" Identity="sitecore\deployment-user" />
</authorization>
```

### Combine Roles and Users

```xml
<authorization>
  <add Permission="Allow" IdentityType="Role" Identity="sitecore\PowerShell Extensions Remoting" />
  <add Permission="Allow" IdentityType="User" Identity="sitecore\admin" />
  <add Permission="Allow" IdentityType="User" Identity="sitecore\build-agent" />
</authorization>
```

## IIS-Level Protection

Add an additional security layer by configuring IIS authentication.

### Deny Anonymous Access

Edit `sitecore modules\PowerShell\Services\web.config`:

```xml
<configuration>
  <system.web>
    <authorization>
      <deny users="?" />
    </authorization>
  </system.web>
</configuration>
```

This denies all anonymous users from accessing SPE web services.

### Windows Authentication

For environments with Windows Authentication:

1. Disable Anonymous Authentication in IIS for `sitecore modules\PowerShell\Services\`
2. Enable Windows Authentication
3. Use the `Credential` parameter in remoting commands:

```powershell
$session = New-ScriptSession -Username "domain\user" -Password "password" -ConnectionUri "https://sitecore.local"
```

## Best Practices

### Security Recommendations

✅ **Do:**
- Only enable services you specifically need
- Require HTTPS for all external services (`requireSecureConnection="true"`)
- Use role-based authorization to limit access
- Deny anonymous access at the IIS level
- Regularly audit which services are enabled
- Use dedicated service accounts for automation
- Monitor web service access logs
- Disable services in Content Delivery (CD) environments

❌ **Don't:**
- Enable all services "just in case"
- Allow HTTP for remoting or file upload
- Grant broad role access (e.g., "Everyone")
- Use administrator accounts for automation
- Enable services on internet-facing servers
- Forget to configure authorization when enabling remoting
- Leave default passwords on service accounts

### Environment-Specific Configurations

| Environment | Recommended Services | Rationale |
|:------------|:---------------------|:----------|
| **Local Dev** | All enabled (if needed) | Convenience for development and testing |
| **Shared Dev** | handleDownload, client, execution | Only UI features |
| **QA/Staging** | Match production | Test with production security |
| **Production CM** | handleDownload, client, execution | Only UI features unless remoting needed |
| **Production CD** | None - don't install SPE | CD servers should not have SPE |

### Decision Matrix: Should You Enable This Service?

| Service | Enable If... | Security Requirements |
|:--------|:-------------|:---------------------|
| **client** | Using Console/ISE (always) | Sitecore user auth + Session Elevation |
| **execution** | Using SPE features (always) | Sitecore user auth |
| **handleDownload** | Need to download reports/files (usually) | Sitecore user auth |
| **remoting** | Need external automation | HTTPS + role auth + IIS deny anonymous |
| **restfulv2** | Building web APIs | HTTPS + script-level security |
| **fileDownload** | Using SPE Remoting file operations | HTTPS + role auth + path restrictions |
| **fileUpload** | Using SPE Remoting file operations | HTTPS + role auth + strict file/path restrictions |
| **mediaDownload** | Using SPE Remoting media operations | HTTPS + role auth |
| **mediaUpload** | Using SPE Remoting media operations | HTTPS + role auth + file type restrictions |
| **restfulv1** | Never (deprecated) | N/A - don't use |

## Troubleshooting

### 403 Forbidden when using requireSecureConnection

**Cause:** Load balancer terminates TLS and forwards HTTP to backend.

**Solution:**
- Configure network-level security instead
- Remove `requireSecureConnection` and secure at network/firewall level
- Or configure load balancer to forward HTTPS

### Remoting returns "Access Denied"

**Possible causes:**
1. User/role not in authorization list
2. IIS denies anonymous access but credentials not provided
3. User doesn't exist or password is incorrect

**Solution:** Check configuration and verify credentials.

### File upload fails even when enabled

**Cause:** File type or location not allowed.

**Solution:** Configure file upload restrictions (see [File Upload Restrictions](file-upload-restrictions.md)).

## Related Topics

- [File Upload Restrictions](file-upload-restrictions.md) - Configure allowed file types and paths
- [IIS Security](iis-security.md) - Additional IIS-level hardening
- [Remoting](../remoting.md) - Using SPE Remoting features
- [Web API](../modules/integration-points/web-api.md) - Building RESTful APIs with SPE

## References

- [SPE Remoting Documentation](../remoting.md)
- [ASP.NET Authorization](https://docs.microsoft.com/en-us/previous-versions/aspnet/wce3kxhd(v=vs.100))