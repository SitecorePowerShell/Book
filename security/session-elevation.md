# Session Elevation (User Account Control)

Session Elevation provides a User Account Control (UAC) layer for SPE, similar to Windows UAC. This security feature requires users to reauthenticate or confirm before accessing powerful SPE features, even if they already have the necessary role permissions.

## Overview

Session Elevation adds a time-based authentication layer on top of Sitecore's role-based security. This provides defense in depth by requiring users to:

1. Have the appropriate Sitecore role (e.g., `sitecore\Developer`)
2. **AND** provide additional authentication or confirmation

This protects against:
- Hijacked sessions being used to execute malicious scripts
- Accidental execution of dangerous operations
- Unauthorized access when a user leaves their workstation unlocked

## How It Works

### Components

Session Elevation consists of three key components:

#### 1. Gates

Entry points where scripts enter Sitecore through built-in interfaces.

**Built-in Gates:**
- **Console** - PowerShell Console application
- **ISE** - Integrated Scripting Environment
- **ItemSave** - Saving PowerShell script items in Content Editor

Each gate can reference a token that controls its elevation requirements.

#### 2. Tokens

Configuration objects that define elevation behavior and session lifetime.

**Token Attributes:**

| Attribute | Description | Values |
|:----------|:------------|:-------|
| `name` | Unique identifier referenced by gates | String (e.g., "Console", "ISE") |
| `expiration` | How long an elevated session lasts | Timespan (hh:mm:ss) |
| `elevationAction` | What happens when elevation is needed | Allow, Block, Password, Confirm |

#### 3. Elevation Actions

The behavior when a user attempts to access a gate without an active elevated session.

## Elevation Actions

### Allow

**Use Case:** Development environments only

Always allows access without prompting. The session runs elevated immediately.

```xml
<token name="Console">
  <patch:attribute name="elevationAction">Allow</patch:attribute>
</token>
```

{% hint style="danger" %}
**Never use Allow in production!** This completely disables session elevation and should only be used on local development machines.
{% endhint %}

### Block

**Use Case:** Completely disable a feature

Always blocks access. The gate cannot be used at all.

```xml
<token name="Console">
  <patch:attribute name="elevationAction">Block</patch:attribute>
</token>
```

**When to use:**
- Disable the Console entirely in production
- Prevent script editing via Content Editor
- Lock down specific gates for security compliance

### Password

**Use Case:** Standard authentication (recommended for most environments)

Prompts the user to enter their password to elevate the session.

```xml
<token name="Console">
  <patch:attribute name="elevationAction">Password</patch:attribute>
  <patch:attribute name="expiration">00:05:00</patch:attribute>
</token>
```

**How it works:**
1. User attempts to access the Console/ISE
2. If no elevated session exists or it has expired, a password prompt appears
3. User enters their Sitecore password
4. Upon successful authentication, session is elevated for the expiration duration
5. During the expiration window, no further prompts appear

![Elevate Session State](../.gitbook/assets/security-elevatedsessionstate-password.png)

**Best for:**
- Standard Sitecore authentication
- Production environments
- QA and staging environments
- Environments with local user accounts

### Confirm

**Use Case:** Single Sign-On (SSO) and federated authentication

Prompts the user to confirm (click a button) to elevate the session. No password is entered.

```xml
<token name="Console">
  <patch:attribute name="elevationAction">Confirm</patch:attribute>
  <patch:attribute name="expiration">00:05:00</patch:attribute>
</token>
```

**When to use:**
- Azure AD authentication
- ADFS/SAML authentication
- Any SSO provider where password re-entry isn't possible
- Identity Server configurations

**Why Confirm instead of Password:**
When using federated authentication, users don't have a password stored in Sitecore. The `Confirm` action provides a "speed bump" to prevent accidental operations without requiring credentials that don't exist.

## Interface Behaviors

### PowerShell Console

When accessing the Console without an elevated session:

1. Password prompt appears (if using Password action)
2. Enter credentials to elevate
3. Console becomes accessible for the token expiration duration

### PowerShell ISE

**Before Elevation:**

The ISE displays a warning banner:

![Elevated session state required](../.gitbook/assets/security-elevatedsessionstate-ise.png)

**After Elevation:**

The warning changes to show the session is elevated with a "Drop" option:

![Drop elevated session state](../.gitbook/assets/security-elevatedsessionstate-dropise.png)

Users can manually drop the elevated session if they finish working early.

### Content Editor

**Before Elevation:**

When editing PowerShell Module, Script Library, or Script items, sensitive fields are hidden and a warning appears:

![Elevate session](../.gitbook/assets/security-elevatedsessionstate-contenteditor.png)

Click **"Elevate session"** to authenticate and reveal the fields.

**After Elevation:**

Fields become editable with an option to drop the session:

![Drop session](../.gitbook/assets/security-elevatedsessionstate-dropcontenteditor.png)

## Configuration Examples

### Development Environment

Relaxed settings for local development:

```xml
<configuration xmlns:patch="https://www.sitecore.net/xmlconfig/">
  <sitecore>
    <powershell>
      <userAccountControl>
        <tokens>
          <token name="Console">
            <patch:attribute name="expiration">01:00:00</patch:attribute>
            <patch:attribute name="elevationAction">Allow</patch:attribute>
          </token>
          <token name="ISE">
            <patch:attribute name="expiration">01:00:00</patch:attribute>
            <patch:attribute name="elevationAction">Allow</patch:attribute>
          </token>
          <token name="ItemSave">
            <patch:attribute name="expiration">01:00:00</patch:attribute>
            <patch:attribute name="elevationAction">Allow</patch:attribute>
          </token>
        </tokens>
      </userAccountControl>
    </powershell>
  </sitecore>
</configuration>
```

### Production Environment (Standard Auth)

Strict settings with password requirement:

```xml
<configuration xmlns:patch="https://www.sitecore.net/xmlconfig/">
  <sitecore>
    <powershell>
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
    </powershell>
  </sitecore>
</configuration>
```

### Production Environment (SSO/Azure AD)

Settings for federated authentication:

```xml
<configuration xmlns:patch="https://www.sitecore.net/xmlconfig/">
  <sitecore>
    <powershell>
      <userAccountControl>
        <tokens>
          <token name="Console">
            <patch:attribute name="expiration">00:05:00</patch:attribute>
            <patch:attribute name="elevationAction">Confirm</patch:attribute>
          </token>
          <token name="ISE">
            <patch:attribute name="expiration">00:05:00</patch:attribute>
            <patch:attribute name="elevationAction">Confirm</patch:attribute>
          </token>
          <token name="ItemSave">
            <patch:attribute name="expiration">00:05:00</patch:attribute>
            <patch:attribute name="elevationAction">Confirm</patch:attribute>
          </token>
        </tokens>
      </userAccountControl>
    </powershell>
  </sitecore>
</configuration>
```

### Disable Console, Enable ISE Only

Lock down the Console while allowing ISE access:

```xml
<configuration xmlns:patch="https://www.sitecore.net/xmlconfig/">
  <sitecore>
    <powershell>
      <userAccountControl>
        <tokens>
          <token name="Console">
            <patch:attribute name="elevationAction">Block</patch:attribute>
          </token>
          <token name="ISE">
            <patch:attribute name="expiration">00:10:00</patch:attribute>
            <patch:attribute name="elevationAction">Password</patch:attribute>
          </token>
          <token name="ItemSave">
            <patch:attribute name="expiration">00:10:00</patch:attribute>
            <patch:attribute name="elevationAction">Password</patch:attribute>
          </token>
        </tokens>
      </userAccountControl>
    </powershell>
  </sitecore>
</configuration>
```

## Advanced Configuration

### Shared Tokens

Multiple gates can share the same token. When a user elevates through one gate, all gates sharing that token are also elevated.

**Example:** Share a single token across all gates:

```xml
<userAccountControl>
  <gates>
    <gate name="Console" token="SharedToken" />
    <gate name="ISE" token="SharedToken" />
    <gate name="ItemSave" token="SharedToken" />
  </gates>
  <tokens>
    <token name="SharedToken">
      <patch:attribute name="expiration">00:10:00</patch:attribute>
      <patch:attribute name="elevationAction">Password</patch:attribute>
    </token>
  </tokens>
</userAccountControl>
```

### Independent Tokens

Each gate can have its own token for fine-grained control:

```xml
<userAccountControl>
  <gates>
    <gate name="Console" token="ConsoleToken" />
    <gate name="ISE" token="ISEToken" />
    <gate name="ItemSave" token="ItemSaveToken" />
  </gates>
  <tokens>
    <token name="ConsoleToken">
      <patch:attribute name="expiration">00:03:00</patch:attribute>
      <patch:attribute name="elevationAction">Password</patch:attribute>
    </token>
    <token name="ISEToken">
      <patch:attribute name="expiration">00:15:00</patch:attribute>
      <patch:attribute name="elevationAction">Password</patch:attribute>
    </token>
    <token name="ItemSaveToken">
      <patch:attribute name="expiration">00:05:00</patch:attribute>
      <patch:attribute name="elevationAction">Confirm</patch:attribute>
    </token>
  </tokens>
</userAccountControl>
```

## Best Practices

### Expiration Times

| Environment | Recommended Expiration | Rationale |
|:------------|:----------------------|:----------|
| **Development** | 30-60 minutes | Minimize interruptions during active development |
| **QA/Staging** | 10-15 minutes | Balance security with testing needs |
| **Production** | 3-5 minutes | Maximum security, minimal exposure window |

### Elevation Actions

| Environment | Recommended Action | Rationale |
|:------------|:------------------|:----------|
| **Local Dev** | Allow | Convenience for solo developers |
| **Shared Dev** | Password | Prevent unauthorized access |
| **QA/Staging** | Password | Match production security |
| **Production (Standard Auth)** | Password | Require reauthentication |
| **Production (SSO/Azure AD)** | Confirm | Provide confirmation step |

### Security Recommendations

✅ **Do:**
- Use short expiration times in production (5 minutes or less)
- Require Password or Confirm in all non-dev environments
- Test your configuration in each environment
- Train users on the "Drop session" functionality
- Document your elevation strategy

❌ **Don't:**
- Use Allow in production
- Set expiration times longer than 15 minutes in production
- Forget to configure all three gates (Console, ISE, ItemSave)
- Ignore the difference between Password and Confirm for SSO

## Troubleshooting

### Password prompt doesn't appear

**Possible causes:**
- Token `elevationAction` is set to `Allow`
- Gate is not configured to use a token
- Configuration patch is not being applied

**Solution:** Verify configuration and check Sitecore logs.

### "Confirm" doesn't work with Azure AD

**Cause:** Configuration shows `Password` instead of `Confirm`.

**Solution:** Change to `elevationAction="Confirm"` for SSO environments.

### Session expires too quickly

**Cause:** Expiration time is too short.

**Solution:** Increase the `expiration` attribute (but keep it under 15 minutes in production).

### Users can't access Console even with correct role

**Cause:** Session elevation is set to `Block` or user doesn't complete elevation.

**Solution:** Check configuration and verify elevation action is appropriate.

## Next Steps

- Configure [Web Services Security](web-services.md) for external access
- Learn about [Delegated Access](delegated-access.md) for controlled privilege escalation
- Review [User and Role Management](users-and-roles.md)
- Complete the [Security Checklist](security-checklist.md)