# IIS-Level Security

Adding security at the IIS (Internet Information Services) level provides an additional layer of protection for SPE web services. This creates defense in depth by blocking unauthorized access before requests even reach the Sitecore application.

## Overview

IIS-level security complements Sitecore-level security by:
- Denying anonymous access to SPE services
- Requiring Windows Authentication
- Blocking access at the web server level
- Protecting against attacks before they reach Sitecore

{% hint style="success" %}
**Defense in Depth:** Even if Sitecore security is bypassed, IIS security provides a second barrier.
{% endhint %}

## SPE Web Services Directory

SPE web services are located in:

```
sitecore modules\PowerShell\Services\
```

**Key Files:**
- `web.config` - IIS configuration for the services directory
- `RemoteAutomation.asmx` - Remoting service
- `RemoteScriptCall.ashx` - RESTful and file services
- `PowerShellWebService.asmx` - Client service (Console/ISE)

## Deny Anonymous Access

The most basic IIS security is denying anonymous users.

### Configuration

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

### How It Works

| User Type | Symbol | Access |
|:----------|:-------|:-------|
| Anonymous | `?` | Denied |
| Authenticated | `*` | Allowed (by default) |

**Effect:**
- All anonymous requests are blocked with 401 Unauthorized
- Users must authenticate with Sitecore credentials
- Protects web services from unauthenticated access

### When to Use

✅ **Always use this configuration** unless you have a specific reason not to.

**Scenarios where you might not:**
- Public API endpoints (rare and dangerous)
- Authentication handled by a different layer

## Windows Authentication

For environments using Windows/Active Directory authentication, you can require Windows credentials at the IIS level.

### Configuration Steps

#### 1. Disable Anonymous Authentication in IIS

1. Open IIS Manager
2. Navigate to `sitecore modules\PowerShell\Services\`
3. Open "Authentication" feature
4. Disable "Anonymous Authentication"
5. Enable "Windows Authentication"

#### 2. Update web.config

Optionally add explicit deny rule:

```xml
<configuration>
  <system.web>
    <authorization>
      <deny users="?" />
    </authorization>
  </system.web>
</configuration>
```

#### 3. Configure Remoting Clients

When using Windows Authentication, SPE Remoting commands require the **Credential** parameter:

```powershell
# Create credential object
$credential = Get-Credential

# Create session with Windows credentials
$session = New-ScriptSession `
    -Username "sitecore\sitecore-user" `
    -Password "password" `
    -ConnectionUri "https://sitecore.local" `
    -Credential $credential

# Or use credential parameter directly
$session = New-ScriptSession `
    -ConnectionUri "https://sitecore.local" `
    -Credential (Get-Credential)
```

### When to Use

**Best for:**
- Internal corporate networks
- Environments with Active Directory
- Servers requiring domain authentication
- High-security environments

**Not suitable for:**
- Internet-facing servers (which shouldn't have SPE anyway)
- Environments without Active Directory
- Mixed authentication scenarios

## Allow Specific Users

Restrict access to specific Windows users or roles.

### Allow Specific Users

```xml
<configuration>
  <system.web>
    <authorization>
      <deny users="?" />
      <allow users="DOMAIN\username1, DOMAIN\username2" />
      <deny users="*" />
    </authorization>
  </system.web>
</configuration>
```

### Allow Specific Roles

```xml
<configuration>
  <system.web>
    <authorization>
      <deny users="?" />
      <allow roles="DOMAIN\Sitecore Administrators" />
      <deny users="*" />
    </authorization>
  </system.web>
</configuration>
```

### Order Matters

Rules are evaluated top to bottom. First match wins.

**Example:**

```xml
<authorization>
  <!-- 1. Deny anonymous -->
  <deny users="?" />

  <!-- 2. Allow specific users -->
  <allow users="DOMAIN\username1" />

  <!-- 3. Allow specific roles -->
  <allow roles="DOMAIN\Sitecore Administrators" />

  <!-- 4. Deny everyone else -->
  <deny users="*" />
</authorization>
```

## IP Address Restrictions

Limit access to specific IP addresses or ranges using IIS IP Address and Domain Restrictions.

### Configuration via IIS Manager

1. Open IIS Manager
2. Navigate to `sitecore modules\PowerShell\Services\`
3. Open "IP Address and Domain Restrictions"
4. Click "Add Allow Entry..." or "Add Deny Entry..."
5. Enter IP address or range

### Configuration via web.config

**Allow specific IPs:**

```xml
<configuration>
  <system.webServer>
    <security>
      <ipSecurity allowUnlisted="false">
        <add ipAddress="10.0.0.1" allowed="true" />
        <add ipAddress="10.0.0.2" allowed="true" />
        <add ipAddress="192.168.1.0" subnetMask="255.255.255.0" allowed="true" />
      </ipSecurity>
    </security>
  </system.webServer>
</configuration>
```

**Deny specific IPs:**

```xml
<configuration>
  <system.webServer>
    <security>
      <ipSecurity allowUnlisted="true">
        <add ipAddress="203.0.113.1" allowed="false" />
        <add ipAddress="198.51.100.0" subnetMask="255.255.255.0" allowed="false" />
      </ipSecurity>
    </security>
  </system.webServer>
</configuration>
```

### When to Use

**Best for:**
- Limiting access to build servers
- Allowing only internal network ranges
- Blocking known malicious IPs
- CI/CD automation from specific servers

**Example Use Case:** Only allow remoting from build server at `10.0.0.100`:

```xml
<system.webServer>
  <security>
    <ipSecurity allowUnlisted="false">
      <add ipAddress="10.0.0.100" allowed="true" />
    </ipSecurity>
  </security>
</system.webServer>
```

## SSL/TLS Requirements

Require HTTPS for all SPE web service access.

### Configuration via web.config

```xml
<configuration>
  <system.webServer>
    <security>
      <access sslFlags="Ssl, SslNegotiateCert" />
    </security>
  </system.webServer>
</configuration>
```

**SSL Flags:**

| Flag | Description |
|:-----|:------------|
| `Ssl` | Require SSL/TLS connection |
| `SslNegotiateCert` | Negotiate client certificate |
| `SslRequireCert` | Require client certificate |
| `Ssl128` | Require 128-bit SSL |

### Basic HTTPS Requirement

```xml
<system.webServer>
  <security>
    <access sslFlags="Ssl" />
  </security>
</system.webServer>
```

### Require Client Certificates

For maximum security, require client certificates:

```xml
<system.webServer>
  <security>
    <access sslFlags="Ssl, SslRequireCert" />
  </security>
</system.webServer>
```

{% hint style="info" %}
**Note:** SPE also has a `requireSecureConnection` setting at the application level. Using both provides defense in depth.
{% endhint %}

## URL Rewrite Rules

Use IIS URL Rewrite module to block or redirect requests.

### Block Suspicious Patterns

```xml
<configuration>
  <system.webServer>
    <rewrite>
      <rules>
        <!-- Block requests with suspicious query strings -->
        <rule name="Block Malicious Query Strings" stopProcessing="true">
          <match url=".*" />
          <conditions>
            <add input="{QUERY_STRING}" pattern="(eval|exec|system|cmd\.exe)" />
          </conditions>
          <action type="CustomResponse" statusCode="403" statusReason="Forbidden" />
        </rule>
      </rules>
    </rewrite>
  </system.webServer>
</configuration>
```

### Redirect HTTP to HTTPS

```xml
<rewrite>
  <rules>
    <rule name="Force HTTPS" stopProcessing="true">
      <match url="(.*)" />
      <conditions>
        <add input="{HTTPS}" pattern="^OFF$" />
      </conditions>
      <action type="Redirect" url="https://{HTTP_HOST}/{R:1}" redirectType="Permanent" />
    </rule>
  </rules>
</rewrite>
```

## Request Filtering

Use IIS Request Filtering to block dangerous requests.

### Block Specific File Extensions

```xml
<configuration>
  <system.webServer>
    <security>
      <requestFiltering>
        <fileExtensions>
          <add fileExtension=".exe" allowed="false" />
          <add fileExtension=".dll" allowed="false" />
          <add fileExtension=".bat" allowed="false" />
          <add fileExtension=".cmd" allowed="false" />
        </fileExtensions>
      </requestFiltering>
    </security>
  </system.webServer>
</configuration>
```

### Limit Request Size

```xml
<system.webServer>
  <security>
    <requestFiltering>
      <!-- Max 10MB requests -->
      <requestLimits maxAllowedContentLength="10485760" />
    </requestFiltering>
  </security>
</system.webServer>
```

### Block HTTP Verbs

```xml
<system.webServer>
  <security>
    <requestFiltering>
      <verbs>
        <add verb="TRACE" allowed="false" />
        <add verb="TRACK" allowed="false" />
        <add verb="OPTIONS" allowed="false" />
      </verbs>
    </requestFiltering>
  </security>
</system.webServer>
```

## Complete Configuration Examples

### Production Environment (Standard Auth)

Strict security with deny anonymous:

```xml
<configuration>
  <system.web>
    <authorization>
      <!-- Deny anonymous access -->
      <deny users="?" />
    </authorization>
  </system.web>

  <system.webServer>
    <security>
      <!-- Require HTTPS -->
      <access sslFlags="Ssl" />

      <!-- Request filtering -->
      <requestFiltering>
        <requestLimits maxAllowedContentLength="10485760" />
        <fileExtensions>
          <add fileExtension=".exe" allowed="false" />
          <add fileExtension=".dll" allowed="false" />
        </fileExtensions>
      </requestFiltering>
    </security>
  </system.webServer>
</configuration>
```

### CI/CD Environment

Allow only build server, require HTTPS:

```xml
<configuration>
  <system.web>
    <authorization>
      <deny users="?" />
    </authorization>
  </system.web>

  <system.webServer>
    <security>
      <!-- Require HTTPS -->
      <access sslFlags="Ssl" />

      <!-- Only allow build server -->
      <ipSecurity allowUnlisted="false">
        <add ipAddress="10.0.0.100" allowed="true" />
      </ipSecurity>
    </security>
  </system.webServer>
</configuration>
```

### Windows Authentication Environment

Require Windows auth with specific roles:

```xml
<configuration>
  <system.web>
    <authorization>
      <deny users="?" />
      <allow roles="DOMAIN\Sitecore Administrators, DOMAIN\Sitecore Developers" />
      <deny users="*" />
    </authorization>
  </system.web>

  <system.webServer>
    <security>
      <access sslFlags="Ssl" />
    </security>
  </system.webServer>
</configuration>
```

### High Security Environment

Multiple layers of protection:

```xml
<configuration>
  <system.web>
    <authorization>
      <deny users="?" />
      <allow users="DOMAIN\sitecore-admin" />
      <deny users="*" />
    </authorization>
  </system.web>

  <system.webServer>
    <security>
      <!-- Require HTTPS and client certificate -->
      <access sslFlags="Ssl, SslRequireCert" />

      <!-- IP restriction to internal network -->
      <ipSecurity allowUnlisted="false">
        <add ipAddress="10.0.0.0" subnetMask="255.0.0.0" allowed="true" />
      </ipSecurity>

      <!-- Request filtering -->
      <requestFiltering>
        <requestLimits maxAllowedContentLength="5242880" />
        <verbs>
          <add verb="TRACE" allowed="false" />
          <add verb="OPTIONS" allowed="false" />
        </verbs>
      </requestFiltering>
    </security>

    <!-- URL rewrite to block patterns -->
    <rewrite>
      <rules>
        <rule name="Block Suspicious Patterns" stopProcessing="true">
          <match url=".*" />
          <conditions>
            <add input="{QUERY_STRING}" pattern="(eval|exec|cmd)" />
          </conditions>
          <action type="CustomResponse" statusCode="403" />
        </rule>
      </rules>
    </rewrite>
  </system.webServer>
</configuration>
```

## Best Practices

### Security Recommendations

✅ **Do:**
- Always deny anonymous access (minimum requirement)
- Require HTTPS for all external access
- Use IP restrictions for CI/CD scenarios
- Implement request filtering to block dangerous patterns
- Layer multiple security controls (defense in depth)
- Test authentication requirements before deploying
- Document your IIS security configuration
- Review IIS logs for unauthorized access attempts

❌ **Don't:**
- Allow anonymous access unless absolutely necessary
- Use HTTP for sensitive operations
- Open access to all IPs without restriction
- Forget to configure both IIS and Sitecore security
- Assume IIS security alone is sufficient
- Deploy without testing authentication workflows

### Configuration Strategy

| Environment | Recommended IIS Security |
|:------------|:------------------------|
| **Local Dev** | Minimal (allow anonymous for convenience) |
| **Shared Dev** | Deny anonymous |
| **QA/Staging** | Deny anonymous + HTTPS |
| **Production** | Deny anonymous + HTTPS + IP restrictions + request filtering |
| **CI/CD** | IP restrictions + HTTPS + specific user/role |

### Defense in Depth Layers

Combine multiple security layers:

1. **Network** - Firewall rules, VPN requirements
2. **IIS** - Authentication, IP restrictions, HTTPS
3. **Sitecore** - Role-based authorization, web service controls
4. **SPE** - Session elevation, file upload restrictions
5. **Monitoring** - Log analysis, alerting

## Troubleshooting

### 401 Unauthorized after configuration

**Possible causes:**
1. Denied anonymous but using anonymous access
2. Windows Authentication not configured correctly
3. User not in allowed users/roles list

**Solution:** Verify authentication method and user permissions.

### 403 Forbidden when accessing services

**Possible causes:**
1. IP address not in allowed list
2. SSL required but using HTTP
3. Client certificate required but not provided

**Solution:** Check IP restrictions, URL protocol, and certificate configuration.

### Windows Authentication doesn't prompt

**Possible causes:**
1. Windows Authentication not enabled in IIS
2. Browser not configured for Windows Auth
3. User in same domain (auto-authentication)

**Solution:** Check IIS authentication settings and browser configuration.

### Works locally but not from remote machine

**Possible causes:**
1. IP restrictions blocking remote IP
2. Firewall blocking access
3. Authentication failing for remote user

**Solution:** Check IP restrictions and firewall rules.

## Related Topics

- [Web Services Security](web-services.md) - Sitecore-level web service configuration
- [Security Policies](security-policies.md) - Understanding SPE security model
- [Security Checklist](security-checklist.md) - Validate your complete security configuration
- [Logging and Monitoring](logging-and-monitoring.md) - Monitor for unauthorized access attempts

## References

- [ASP.NET Authorization](https://docs.microsoft.com/en-us/previous-versions/aspnet/wce3kxhd(v=vs.100))
- [IIS IP Security](https://docs.microsoft.com/en-us/iis/configuration/system.webserver/security/ipsecurity/)
- [IIS Request Filtering](https://docs.microsoft.com/en-us/iis/configuration/system.webserver/security/requestfiltering/)
- [IIS URL Rewrite](https://docs.microsoft.com/en-us/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference)