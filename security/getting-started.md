# Getting Started with SPE Security

This guide helps you quickly secure your Sitecore PowerShell Extensions installation with essential security configurations.

## Critical Security Warnings

{% hint style="danger" %}
**Never install SPE in internet-facing environments!**

- DO NOT install on Content Delivery (CD) instances
- DO NOT deploy on servers facing the Internet
- DO NOT expose SPE endpoints to untrusted networks

SPE is a powerful development and administration tool intended for **Content Management (CM)** servers in **protected internal networks** only.
{% endhint %}

## Quick Security Checklist

Before deploying SPE to any environment beyond your local development machine, complete these essential steps:

### ✓ Environment Assessment

- [ ] Confirm this is a Content Management (CM) server
- [ ] Verify the server is NOT internet-facing
- [ ] Ensure the server is behind a firewall
- [ ] Confirm this is NOT a Content Delivery (CD) instance

### ✓ Web Services Security

By default, all SPE web services are **disabled** except those required for the Sitecore UI. Keep them disabled unless you have a specific need.

- [ ] Review enabled services in `App_Config\Include\Spe\Spe.config`
- [ ] Only enable services you specifically need
- [ ] Configure HTTPS for any enabled external services
- [ ] Review the [Web Services](web-services.md) documentation

### ✓ User Access Control

- [ ] Review default role permissions (under `core:\content\Applications\PowerShell`)
- [ ] Ensure only trusted administrators have access
- [ ] Enable Session Elevation (UAC) for production environments
- [ ] Configure appropriate token expiration times
- [ ] Review the [Session Elevation](session-elevation.md) documentation

### ✓ Application Pool Security

- [ ] Review the application pool service account permissions
- [ ] Ensure the account follows the principle of least privilege
- [ ] Verify file system access is restricted to necessary directories only
- [ ] Consider using a named service account instead of ApplicationPoolIdentity

### ✓ Content Editor Security

- [ ] Verify PowerShell Console visibility is restricted to `sitecore\Developer` role
- [ ] Verify PowerShell ISE visibility is restricted to `sitecore\Developer` role
- [ ] Review context menu security settings
- [ ] Test that non-privileged users cannot access SPE features

## Initial Configuration Steps

### Step 1: Review Default Settings

The default security configuration is found in:
- `App_Config\Include\Spe\Spe.config`

**Do not modify this file directly.** Instead, create configuration patch files.

### Step 2: Create Your Security Patch

Create a new configuration file: `App_Config\Include\Spe\Custom\Spe.Custom.config`

**Example:** Basic production security configuration:

```xml
<configuration xmlns:patch="https://www.sitecore.net/xmlconfig/">
  <sitecore>
    <powershell>
      <!-- Session Elevation (UAC) -->
      <userAccountControl>
        <tokens>
          <token name="Console">
            <patch:attribute name="expiration">00:05:00</patch:attribute>
            <patch:attribute name="elevationAction">Password</patch:attribute>
          </token>
          <token name="ISE">
            <patch:attribute name="expiration">00:05:00</patch:attribute>
            <patch:attribute name="elevationAction">Password</patch:attribute>
          </token>
          <token name="ItemSave">
            <patch:attribute name="expiration">00:05:00</patch:attribute>
            <patch:attribute name="elevationAction">Password</patch:attribute>
          </token>
        </tokens>
      </userAccountControl>

      <!-- Ensure web services remain disabled -->
      <services>
        <remoting enabled="false" />
        <restfulv1 enabled="false" />
        <restfulv2 enabled="false" />
        <fileDownload enabled="false" />
        <fileUpload enabled="false" />
        <mediaDownload enabled="false" />
        <mediaUpload enabled="false" />
      </services>
    </powershell>
  </sitecore>
</configuration>
```

### Step 3: Configure IIS Authentication

Protect the SPE services directory at the IIS level.

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

This denies anonymous access to all SPE web services.

### Step 4: Test Your Configuration

1. Log in as a non-administrator user
2. Verify you cannot access the PowerShell Console
3. Verify you cannot access the PowerShell ISE
4. Log in as an administrator
5. Verify Session Elevation prompts appear when expected
6. Test that scripts execute successfully after elevation

## Environment-Specific Recommendations

### Development Environment

For local development machines, you may use relaxed settings:

```xml
<token name="Console">
  <patch:attribute name="expiration">01:00:00</patch:attribute>
  <patch:attribute name="elevationAction">Allow</patch:attribute>
</token>
```

{% hint style="warning" %}
**Never use `elevationAction="Allow"` in non-development environments!**
{% endhint %}

### QA/Staging Environment

Use the same strict security as production, but you may extend session timeouts slightly for testing convenience:

```xml
<token name="Console">
  <patch:attribute name="expiration">00:15:00</patch:attribute>
  <patch:attribute name="elevationAction">Password</patch:attribute>
</token>
```

### Production Environment

Use the strictest settings:

```xml
<token name="Console">
  <patch:attribute name="expiration">00:05:00</patch:attribute>
  <patch:attribute name="elevationAction">Password</patch:attribute>
</token>
```

For Azure AD/SSO environments, use:

```xml
<token name="Console">
  <patch:attribute name="elevationAction">Confirm</patch:attribute>
</token>
```

## Identity Server Configuration (Sitecore 9.1+)

If using Sitecore 9.1 or later with Identity Server, enable this configuration file:

**File:** `App_Config\Include\Spe\Spe.IdentityServer.config`

This prevents infinite loops in the SPE Console when using OWIN cookie authentication.

## Next Steps

After completing the initial setup:

1. Review the [Security Policies](security-policies.md) to understand the security model
2. Configure [Session Elevation](session-elevation.md) for your environment
3. If you need external access, carefully review [Web Services](web-services.md) security
4. Learn about [User and Role Management](users-and-roles.md)
5. Complete the [Security Checklist](security-checklist.md) before deployment

## Common Mistakes to Avoid

❌ **Don't install on CD servers** - SPE is for CM only
❌ **Don't expose to the internet** - Keep SPE behind firewalls
❌ **Don't use Allow elevation in production** - Always require password or confirmation
❌ **Don't enable unnecessary web services** - Only enable what you specifically need
❌ **Don't grant broad access** - Limit to administrators only
❌ **Don't skip session elevation** - UAC is critical for production environments

## Getting Help

If you need assistance with SPE security:

- Review the detailed [Security Hardening](README.md) documentation
- Check the [Security Checklist](security-checklist.md)
- Visit the [GitHub repository](https://github.com/SitecorePowerShell/Console)
- Join #module-spe on Sitecore Community Slack
