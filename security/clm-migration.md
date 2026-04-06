# CLM Migration Guide

{% hint style="info" %}
Introduced in SPE 9.0.
{% endhint %}

This guide helps existing SPE installations adopt Constrained Language Mode (CLM) restriction profiles safely. CLM is fully opt-in — upgrading to SPE 9.0 introduces **zero behavior changes** until you explicitly configure a profile.

## Key Message: No Breaking Changes

- All services default to the `unrestricted` profile (FullLanguage, no restrictions)
- Existing remoting scripts continue to work unchanged after upgrading
- CLM features activate only when an administrator sets a profile on a service or API Key
- Pipeline scripts (LoggedIn, LoggingIn, Logout) always run in FullLanguage regardless of profile

## What Changes for Existing Scripts

| Aspect | Before 9.0 | After 9.0 (default) | After 9.0 (with profile) |
| :--- | :--- | :--- | :--- |
| Language mode | FullLanguage | FullLanguage | Per profile |
| Command access | All | All | Filtered by profile |
| .NET types | Unrestricted | Unrestricted | Blocked in ConstrainedLanguage |
| Item access | All paths | All paths | Filtered by path restrictions |
| Auth method | Shared secret / JWT | Same (unchanged) | Same + API Keys |

## Recommended Rollout Steps

### Step 1: Upgrade and Verify

Install the SPE 9.0 package and verify existing functionality:

- Confirm all remoting scripts continue to work unchanged
- No configuration changes are needed at this stage
- The `unrestricted` profile is active by default

### Step 2: Enable Audit Mode (Dry Run)

Set a profile on the remoting service with `enforcement="Audit"` to log what **would** be blocked without actually blocking anything:

```xml
<configuration xmlns:patch="https://www.sitecore.net/xmlconfig/">
  <sitecore>
    <powershell>
      <services>
        <remoting>
          <patch:attribute name="profile">read-only</patch:attribute>
          <patch:attribute name="enforcement">Audit</patch:attribute>
        </remoting>
      </services>
    </powershell>
  </sitecore>
</configuration>
```

Monitor Sitecore logs for `[Security]` violation entries:

```powershell
Get-Content "$SitecoreLogFolder\SPE.log.*.txt" | Where-Object { $_ -match "\[Security\].*action=.*Blocked" }
```

### Step 3: Remediate Scripts

Review audit log violations and address them:

- **Replace blocked commands** with allowed alternatives where possible
- **Register essential scripts** as [trusted](trusted-scripts.md) if they legitimately need CLM bypass
- **Create item-based overrides** if built-in profiles need customization
- **Use `New-PSObject`** instead of `[PSCustomObject]@{}` for CLM-safe object creation

### Step 4: Switch to Enforce Mode

Once audit logs show no unexpected violations, enable enforcement:

```xml
<configuration xmlns:patch="https://www.sitecore.net/xmlconfig/">
  <sitecore>
    <powershell>
      <services>
        <remoting>
          <patch:attribute name="profile">read-only</patch:attribute>
          <patch:attribute name="enforcement">Enforce</patch:attribute>
        </remoting>
      </services>
    </powershell>
  </sitecore>
</configuration>
```

Monitor for 403 responses from remoting clients and adjust trusted scripts or profile overrides as needed.

### Step 5: Migrate to API Keys (Optional)

For per-consumer security, create [API Key](api-keys.md) items:

1. Create API Key items at `/sitecore/system/Modules/PowerShell/Settings/Remoting/API Keys/`
2. Assign appropriate profiles per consumer
3. Configure throttling for external consumers
4. Migrate clients from the shared secret to per-key secrets

## Troubleshooting

### 403 Forbidden with X-SPE-BlockedCommand

A command is blocked by the active profile. Check the `X-SPE-BlockedCommand` response header to identify which command, and `X-SPE-Profile` for the profile name.

**Options:**
- Use an alternative command not on the blocklist
- Register the script as [trusted](trusted-scripts.md)
- Add the command to the profile's allowlist via [item-based overrides](restriction-profiles.md#item-based-overrides)

### 429 Too Many Requests

An API Key's rate limit has been exceeded. The `Retry-After` header indicates how long to wait. Increase the key's `RequestLimit` or `ThrottleWindow` if the limit is too restrictive.

### Scripts Failing Silently

If scripts appear to work but produce unexpected results, check audit logs for violations that were logged but not enforced (audit mode). Switch to enforce mode to surface these as explicit errors.

### Trusted Script Not Working

- Verify the trust item is **Enabled**
- Verify the `AllowedProfiles` field includes the active profile (or is empty for all profiles)
- Verify the Treelist field references the correct script items
- Check cache — trust items have TTL-based caching

## Related Topics

- [Restriction Profiles](restriction-profiles.md) — Profile definitions and features
- [API Keys](api-keys.md) — Per-consumer credentials
- [Trusted Scripts](trusted-scripts.md) — Bypass CLM for specific scripts
- [Item Path Restrictions](item-path-restrictions.md) — Content tree access control
- [Logging and Monitoring](logging-and-monitoring.md) — Audit log analysis
