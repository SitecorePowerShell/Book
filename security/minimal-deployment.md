# Minimal Web Service Deployment

For CI/CD and automation environments where you only need SPE web services (without the full UI), a minimal deployment reduces the attack surface while maintaining remote automation capabilities.

## Overview

The minimal deployment includes only the files necessary to support SPE web services, removing all UI components (Console, ISE, Content Editor integrations).

**Benefits:**

- Smaller attack surface
- Fewer files to maintain
- Faster deployment
- Ideal for build/deployment servers
- Only what's needed for automation

**Limitations:**

- No PowerShell Console
- No PowerShell ISE
- No Content Editor integration
- No Reports UI
- Web services only (Remoting)

{% hint style="info" %}
This deployment is specifically designed for environments that only need remote script execution via SPE Remoting.
{% endhint %}

## Use Cases

### CI/CD Environments

**Scenario:** Build servers need to execute deployment scripts remotely.

**Requirements:**

- Remoting service
- Minimal file footprint
- No interactive UI

### Automated Content Deployment

**Scenario:** Automated systems deploy content packages to Sitecore.

**Requirements:**

- Remote script execution
- File upload capability
- Package installation commands

### Headless/API-Only Instances

**Scenario:** Sitecore instances that serve as API backends without content authoring.

**Requirements:**

- Remote automation capability
- No UI overhead
- Security-focused configuration

## Required Files

### Core Files

| File Path                                   | Purpose                   | Required |
| :------------------------------------------ | :------------------------ | :------- |
| `App_Config\Include\Spe\Spe.config`         | Core configuration        | ✓ Yes    |
| `App_Config\Include\Spe\Spe.Minimal.config` | Minimal deployment config | ✓ Yes    |
| `bin\Spe.dll`                               | Main assembly             | ✓ Yes    |
| `bin\Spe.Abstractions.dll`                  | Abstractions assembly     | ✓ Yes    |

### Web Service Files

| File Path                                                    | Purpose               | Required |
| :----------------------------------------------------------- | :-------------------- | :------- |
| `sitecore modules\PowerShell\Services\web.config`            | IIS configuration     | ✓ Yes    |
| `sitecore modules\PowerShell\Services\RemoteAutomation.asmx` | Remoting service      | ✓ Yes    |
| `sitecore modules\PowerShell\Services\RemoteScriptCall.ashx` | RESTful/file services | ✓ Yes    |

### Excluded Files (Not Needed)

The following are NOT included in minimal deployment:

❌ PowerShell Console UI files
❌ PowerShell ISE files
❌ Content Editor integration files
❌ Report UI files
❌ Gutter renderers
❌ Ribbon extensions
❌ All UI-related files in `sitecore modules\Shell\PowerShell\`

## Installation Steps

### Option 1: Using Minimal Package

SPE provides a pre-built minimal package: `SPE.Minimal-6.x.zip`

1. Download `SPE.Minimal-8.x.zip` from SPE releases
2. Extract to your Sitecore instance
3. Enable disabled config files (see below)
4. Configure security settings
5. Test connectivity

### Option 2: Manual Installation

Deploy required files using your favorite scripting language.

#### Step 3: Disable UI Control Sources

Create a config patch to disable UI control sources:

**File:** `App_Config\Include\Spe\Custom\Spe.DisableUI.config`

```xml
<configuration xmlns:patch="https://www.sitecore.net/xmlconfig/">
    <sitecore>
        <controlSources>
            <source mode="on" namespace="Spe.Client.Controls" assembly="Spe">
                <patch:delete />
            </source>
            <source mode="on" namespace="Spe.Client.Applications"
                  folder="/sitecore modules/Shell/PowerShell/" deep="true">
                <patch:delete />
            </source>
        </controlSources>
    </sitecore>
</configuration>
```

## Configuration

### Enable Remoting Service

By default, Remoting is **disabled**. Enable it with proper security:

**File:** `App_Config\Include\Spe\Custom\Spe.Remoting.config`

```xml
<configuration xmlns:patch="https://www.sitecore.net/xmlconfig/">
  <sitecore>
    <powershell>
      <services>
        <!-- Enable remoting with HTTPS requirement -->
        <remoting>
          <patch:attribute name="enabled">true</patch:attribute>
          <patch:attribute name="requireSecureConnection">true</patch:attribute>

          <!-- Clear default authorization -->
          <authorization>
            <patch:delete />
          </authorization>

          <!-- Add specific service account -->
          <authorization>
            <add Permission="Allow" IdentityType="User" Identity="sitecore\automation-user" desc="CI/CD automation account" />
          </authorization>
        </remoting>

        <!-- Optionally enable file operations for package deployment -->
        <fileDownload>
          <patch:attribute name="enabled">true</patch:attribute>
        </fileDownload>

        <fileUpload>
          <patch:attribute name="enabled">true</patch:attribute>
        </fileUpload>
      </services>
    </powershell>
  </sitecore>
</configuration>
```

### Secure Web Services

**File:** `sitecore modules\PowerShell\Services\web.config`

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

      <!-- Optional: IP restrictions for build servers -->
      <ipSecurity allowUnlisted="false">
        <add ipAddress="10.0.0.100" allowed="true" desc="Build Server 1" />
        <add ipAddress="10.0.0.101" allowed="true" desc="Build Server 2" />
      </ipSecurity>
    </security>
  </system.webServer>
</configuration>
```

## Testing the Deployment

### Verify Minimal Installation

```powershell
# Test from remote machine using SPE Remoting module

Import-Module SPE

# Create session
$session = New-ScriptSession `
    -Username "sitecore\automation-user" `
    -Password "SecurePassword123!" `
    -ConnectionUri "https://sitecore-instance.local"

# Test script execution
$result = Invoke-RemoteScript -Session $session -ScriptBlock {
    Get-Item -Path "master:\content\home" | Select-Object -ExpandProperty Name
}

Write-Host "Result: $result" -ForegroundColor Green

# Close session
Stop-ScriptSession -Session $session
```

## Security Best Practices

### Minimal Deployment Security Checklist

- [ ] **Remoting disabled by default** - Only enable when configuration complete
- [ ] **HTTPS required** (`requireSecureConnection="true"`)
- [ ] **Anonymous access denied** in web.config
- [ ] **Specific user accounts** configured (not Admin)
- [ ] **IP restrictions** configured (if applicable)
- [ ] **File upload restrictions** properly configured
- [ ] **Service accounts** use strong passwords
- [ ] **Service accounts** have minimal Sitecore permissions
- [ ] **Logging enabled** for all remote operations
- [ ] **Regular log review** scheduled
- [ ] **UI components verified disabled**

### Recommended Security Layers

1. **Network** - Firewall rules, VPN if possible
2. **IIS** - IP restrictions, Windows Auth (optional), HTTPS
3. **Sitecore** - Specific user accounts with minimal roles
4. **SPE** - requireSecureConnection, file upload restrictions
5. **Monitoring** - Comprehensive logging and alerts

## Troubleshooting

### Remoting connection fails

**Possible causes:**

1. Remoting service not enabled
2. User not in authorization list
3. HTTPS required but using HTTP
4. Anonymous access denied but not providing credentials
5. IP address blocked

**Solution:** Check configuration, verify credentials, ensure HTTPS.

### UI elements still appear

**Cause:** controlSources not disabled in configuration.

**Solution:** Verify `Spe.DisableUI.config` patch is present and loading.

## Related Topics

- [Web Services Security](web-services.md) - Detailed web service configuration
- [IIS Security](iis-security.md) - IIS-level hardening
- [File Upload Restrictions](file-upload-restrictions.md) - Configure upload limits
- [Security Checklist](security-checklist.md) - Validation checklist
- [Logging and Monitoring](logging-and-monitoring.md) - Monitoring remote operations
- [Remoting](../remoting.md) - Using SPE Remoting features

## References

- [SPE Remoting Documentation](../remoting.md)
- [SPE Minimal Package Download](https://github.com/SitecorePowerShell/Console/releases)
