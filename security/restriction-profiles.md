# Restriction Profiles

{% hint style="info" %}
Introduced in SPE 9.0.
{% endhint %}

Restriction profiles provide tiered security for SPE remoting endpoints. Each profile defines what a remote session can do — which PowerShell language features are available, which commands can run, which modules can load, and which Sitecore content paths are accessible.

{% hint style="warning" %}
Restriction profiles apply only to remoting endpoints. The ISE, Console, and other SPE integration points are not affected.
{% endhint %}

## Built-in Profiles

SPE ships with four predefined profiles:

| Profile | Language Mode | Command Mode | Use Case |
| :--- | :--- | :--- | :--- |
| `unrestricted` | FullLanguage | No restrictions | Default — backwards compatible, full access |
| `read-only` | ConstrainedLanguage | Blocklist | Reporting, dashboards, read-only integrations |
| `read-only-strict` | ConstrainedLanguage | Allowlist (strict) | Untrusted consumers with minimal access |
| `content-editor` | ConstrainedLanguage | Allowlist | Content management APIs with scoped paths |

## Profile Features

### Language Mode

Profiles set the PowerShell language mode for the remote session:

- **FullLanguage** — No restrictions. All PowerShell features available including .NET type access and method calls.
- **ConstrainedLanguage** — Blocks arbitrary .NET type access, dynamic method calls, and `[PSCustomObject]@{}` construction syntax. Use `New-PSObject` as a CLM-safe alternative.

### Command Restrictions

| Mode | Behavior |
| :--- | :--- |
| **Blocklist** | All commands allowed except those explicitly blocked. Use when most commands are safe. |
| **Allowlist** | All commands blocked except those explicitly allowed. Use for maximum restriction. |

### Module Restrictions

Control which PowerShell modules can be loaded in remote sessions. Configure autoload preferences (`None` or `All`) per profile.

### Item Path Restrictions

Control which Sitecore content paths are accessible. See [Item Path Restrictions](item-path-restrictions.md) for details.

### Audit Level

| Level | Description |
| :--- | :--- |
| `None` | No audit logging. |
| `Violations` | Log only blocked operations. |
| `Standard` | Log violations and key operations. |
| `Full` | Log all operations for complete audit trail. |

### Enforcement Mode

| Mode | Description |
| :--- | :--- |
| `Enforce` | Block violations and return errors. |
| `Audit` | Log violations but allow execution. Use for dry-run rollout. |

## Profile Resolution Order

When a remoting request arrives, the active profile is determined by:

1. **JWT `scope` claim** — highest precedence
2. **API Key item profile** — if API key authentication is used
3. **Service-level profile** — from the `profile` attribute in `Spe.config`
4. **`unrestricted`** — default fallback

Unknown profile names resolve to **DenyAll** (fail closed).

## Configuration

Profiles are defined in `Spe.config` under `<restrictionProfiles>`. Assign a profile to a service using the `profile` attribute:

```xml
<configuration xmlns:patch="https://www.sitecore.net/xmlconfig/">
  <sitecore>
    <powershell>
      <services>
        <remoting>
          <patch:attribute name="enabled">true</patch:attribute>
          <patch:attribute name="profile">read-only</patch:attribute>
        </remoting>
      </services>
    </powershell>
  </sitecore>
</configuration>
```

## Item-Based Overrides

Config-defined profiles can be extended through Sitecore content items at:

```
/sitecore/system/Modules/PowerShell/Settings/Remoting/Restriction Profiles/
```

Override items use the `Restriction Profile` template and support nested folders for organization.

| Field | Description |
| :--- | :--- |
| `Enabled` | Activate or deactivate this override. |
| `BaseProfile` | The config-defined profile to extend. |
| `AdditionalBlockedCommands` | Commands to add to the blocklist. |
| `AdditionalAllowedCommands` | Commands to add to the allowlist. |
| `AdditionalBlockedPaths` | Sitecore paths to block (Treelist). |
| `AdditionalAllowedPaths` | Sitecore paths to allow (Treelist). |
| `AuditLevelOverride` | Override the audit level for this profile. |

{% hint style="info" %}
Overrides are **additive only** — they can add restrictions but cannot remove base profile restrictions. The most restrictive setting always wins.
{% endhint %}

Override items are cached with TTL-based expiry and automatically invalidated when saved.

## Response Headers

When a restriction profile is active, remoting responses include:

| Header | Description |
| :--- | :--- |
| `X-SPE-Profile` | The resolved profile name. |
| `X-SPE-LanguageMode` | The active language mode (`FullLanguage` or `ConstrainedLanguage`). |
| `X-SPE-Restriction` | Set on 403 responses when a command is blocked. |
| `X-SPE-BlockedCommand` | The specific command that was blocked. |

## Related Topics

- [API Keys](api-keys.md) — Per-consumer credentials with profile binding
- [Trusted Scripts](trusted-scripts.md) — Allow specific scripts to bypass CLM
- [Item Path Restrictions](item-path-restrictions.md) — Control content tree access
- [Web Services](web-services.md) — Service-level profile assignment
- [CLM Migration Guide](clm-migration.md) — Rollout steps for existing installations
