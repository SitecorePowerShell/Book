# Security Policies

Understanding the security policies that govern SPE is essential for properly securing your installation. SPE operates under two primary security contexts that work together to control what scripts can do.

## Overview

SPE security operates on two levels:

1. **Application Pool Service Account** - Controls OS-level access (file system, registry, network)
2. **Sitecore User Account** - Controls Sitecore API access (content, security, configuration)

Both security contexts work together to determine what a PowerShell script can accomplish.

## Application Pool Service Account

### How It Works

The Windows PowerShell runspace executes under the identity of the IIS application pool service account. This account determines what OS-level resources the script can access through PowerShell providers.

**Key Providers Affected:**
- **FileSystem** - Read/write files and directories
- **Registry** - Read/write Windows registry keys
- **Environment** - Access environment variables
- **Certificate** - Access certificate stores

### Security Implications

{% hint style="warning" %}
The application pool service account gives SPE access to OS-level features. If the service account can delete files from the root directory, SPE can do the same.
{% endhint %}

**Example:** If your application pool runs as `NETWORK SERVICE` or `ApplicationPoolIdentity`, the script may have:
- Read/write access to `C:\inetpub\wwwroot`
- Access to the application directories
- Potentially access to other directories on the system

### Common Service Account Types

| Service Account | Typical Permissions | Security Considerations |
|:----------------|:--------------------|:------------------------|
| **ApplicationPoolIdentity** | Limited local access | No user profile (`$HOME` is empty); may have broader file system access than expected |
| **NetworkService** | Network and local access | Similar to ApplicationPoolIdentity; verify file system permissions |
| **Custom Domain Account** | Defined by domain policy | Has a user profile; permissions controlled by Active Directory |
| **LocalSystem** | Full system access | ❌ **Never use** - grants unlimited access |

### Best Practices

✅ **Do:**
- Use the principle of least privilege
- Audit file system permissions for the service account
- Restrict the account to only necessary directories
- Use a named service account when you need specific permissions
- Test what directories the account can access

❌ **Don't:**
- Run as LocalSystem or Administrator
- Grant the service account unnecessary file system permissions
- Assume ApplicationPoolIdentity is fully restricted
- Allow write access to system directories

### Verifying Service Account Access

**Example:** Check which directories the service account can access:

```powershell
# Test file system access
Test-Path "C:\Windows\System32" -PathType Container
Test-Path "C:\inetpub\wwwroot" -PathType Container

# Check current identity
[System.Security.Principal.WindowsIdentity]::GetCurrent().Name

# Verify HOME directory
$HOME
```

### Restricting File System Access

You can restrict which directories SPE can access by:

1. **NTFS Permissions** - Remove unnecessary file system permissions from the service account
2. **AppLocker** - Use Windows AppLocker to restrict PowerShell execution paths
3. **File Upload Restrictions** - Configure SPE to only allow uploads to specific locations (see [Web Services](web-services.md))

## Sitecore User Account

### How It Works

Scripts execute within the security context of the logged-in Sitecore user. The user's Sitecore roles and permissions determine what content and functionality the script can access through the Sitecore API.

**Key Areas Controlled:**
- Content item access (read/write/delete)
- Security management (users/roles)
- Configuration access
- Workflow operations
- Publishing operations

### Security Implications

The Sitecore security model applies to all script operations that use the Sitecore API:

```powershell
# This respects Sitecore security
Get-Item -Path "master:\content\home"

# This also respects Sitecore security
Get-User -Identity "sitecore\testuser"
```

However, scripts can bypass Sitecore security just like any other Sitecore API code:

```powershell
# This bypasses Sitecore security
New-UsingBlock (New-Object Sitecore.SecurityModel.SecurityDisabler) {
    # Code here runs with full Sitecore permissions
    Get-Item -Path "master:\content\home"
}
```

{% hint style="danger" %}
**Security Note:** Scripts can disable Sitecore security checks using `SecurityDisabler` and other disablers. This is why limiting who can write and execute scripts is critical.
{% endhint %}

### Application-Level Security

SPE features are protected by Sitecore security at the application level. Access to SPE interfaces is controlled through item-level security in the Core database.

**Configuration Location:** `core:\content\Applications\PowerShell`

| Feature | Default Visibility | Security Path |
|:--------|:-------------------|:--------------|
| **PowerShell Console** | `sitecore\Developer` (read) | `core:\content\Applications\PowerShell\PowerShellConsole` |
| **PowerShell ISE** | `sitecore\Developer` (read) | `core:\content\Applications\PowerShell\PowerShellIse` |
| **PowerShell ListView** | `sitecore\Sitecore Client Users` (read) | `core:\content\Applications\PowerShell\PowerShellListView` |
| **PowerShell Runner** | `sitecore\Sitecore Client Users` (read) | `core:\content\Applications\PowerShell\PowerShellRunner` |
| **PowerShell Reports** | `sitecore\Sitecore Client Authoring` (read) | See [Reports documentation](../modules/integration-points/reports/) |

**Note:** Security is validated in each SPE application within the `OnLoad` function.

### Menu Item Security

Context menu items in the Content Editor are also protected by security.

**Configuration Location:** `core:\content\Applications\Content Editor\Context Menues\Default\`

| Feature | Visibility | Command State |
|:--------|:-----------|:--------------|
| **Edit with ISE** | `sitecore\Developer` (read) | **Enabled** when item template is _PowerShell Script_, otherwise **Hidden** |
| **Console** | `sitecore\Developer` (read) | **Enabled** when user is _admin_ or in role _sitecore\Sitecore Client Developing_, otherwise **Hidden** |
| **Scripts** | `sitecore\Sitecore Limited Content Editor` (deny read) | **Enabled** when the service and user are authorized to execute, otherwise **Hidden** |

**Note:** PowerShell Script Library and PowerShell Script items have additional visibility and enabled rules. Adjust role permissions on these items to control menu visibility.

### Best Practices

✅ **Do:**
- Only grant SPE access to trusted administrators
- Use Sitecore roles to control feature access
- Require Session Elevation (UAC) for production environments
- Audit which users have access to Developer and related roles
- Review script libraries for appropriate security settings
- Use delegated access for controlled privilege escalation

❌ **Don't:**
- Grant `sitecore\Developer` role to content authors
- Assume Sitecore security prevents all unauthorized actions
- Allow untrusted users to write or execute scripts
- Ignore the fact that scripts can disable security
- Give broad access to SPE features

### Verifying User Access

**Example:** Check current user context and permissions:

```powershell
# Get current user
$user = Get-User -Current
Write-Host "Current User: $($user.Name)"

# Check if user is in a role
$user.IsInRole("sitecore\Developer")

# List all roles for current user
$user.Roles | ForEach-Object { $_.Name }

# Test item access
$item = Get-Item -Path "master:\content\home"
$item.Access.CanRead()
$item.Access.CanWrite()
$item.Access.CanDelete()
```

## Combined Security Model

Both security contexts work together. A script is limited by whichever context is more restrictive:

**Example Scenarios:**

| Scenario | Service Account | User Account | Result |
|:---------|:----------------|:-------------|:-------|
| Read Sitecore content | Has file access | Has read permission | ✅ Success |
| Read Sitecore content | Has file access | No read permission | ❌ Denied by Sitecore |
| Delete file outside Sitecore | No file permission | Is administrator | ❌ Denied by Windows |
| Delete file outside Sitecore | Has file permission | Is administrator | ✅ Success (dangerous!) |

## Least Privilege Strategy

Implement defense in depth by restricting both security contexts:

### Service Account
- Minimum file system access
- No access to sensitive directories
- Regular permission audits

### Sitecore User Account
- Role-based access control
- Session elevation required
- Regular role membership audits
- Script library security reviews

### Additional Layers
- [Session Elevation (UAC)](session-elevation.md) - Require reauthentication
- [Web Services Security](web-services.md) - Disable unnecessary endpoints
- [IIS-Level Security](iis-security.md) - Block anonymous access
- [Monitoring and Logging](logging-and-monitoring.md) - Detect unauthorized access

## Next Steps

- Configure [Session Elevation](session-elevation.md) to add an additional authentication layer
- Review [Web Services Security](web-services.md) if you need external access
- Learn about [User and Role Management](users-and-roles.md)
- Complete the [Security Checklist](security-checklist.md)

## References

- [Sitecore Security API Documentation](https://doc.sitecore.com/xp/en/developers/latest/platform-administration-and-architecture/security-api.html)
- [IIS Application Pool Identities](https://docs.microsoft.com/en-us/iis/manage/configuring-security/application-pool-identities)