# API Keys

{% hint style="info" %}
Introduced in SPE 9.0.
{% endhint %}

Remoting API Keys provide per-consumer credentials that bind a shared secret to a [restriction profile](restriction-profiles.md). Each key can have its own profile, user impersonation, and rate limits — replacing the single shared secret approach with granular, auditable access control.

{% hint style="warning" %}
**Prefer API Keys over the legacy shared secret.** API Keys provide per-consumer profiles, rate limiting, and audit trails. See [Remoting Authentication](../remoting.md#authentication) for an overview.
{% endhint %}

## Overview

API Keys are managed as Sitecore content items at:

```
/sitecore/system/Modules/PowerShell/Settings/Remoting/API Keys/
```

Each key uses the `Remoting API Key` template and supports nested folders for organization.

## API Key Fields

| Field | Type | Description |
| :--- | :--- | :--- |
| `SharedSecret` | Single-Line Text | Authentication credential. Compared using constant-time comparison to prevent timing attacks. |
| `Enabled` | Checkbox | Activate or deactivate this key. |
| `Profile` | Single-Line Text | [Restriction profile](restriction-profiles.md) to apply for sessions using this key. |
| `ImpersonateUser` | Single-Line Text | Sitecore user to impersonate (e.g., `sitecore\mcp`). |
| `RequestLimit` | Integer | Maximum requests allowed within the throttle window. |
| `ThrottleWindow` | Integer | Throttle window duration in seconds. |

## Setup

1. Navigate to `/sitecore/system/Modules/PowerShell/Settings/Remoting/API Keys/`
2. Create a new item using the `Remoting API Key` template
3. Set a unique shared secret
4. Select a restriction profile
5. Optionally configure user impersonation and throttling
6. Use the shared secret in client `New-ScriptSession` calls

## Authentication Flow

When a remoting request arrives:

1. The shared secret is matched against API Key items first
2. If a match is found, the key's profile and impersonation settings are applied
3. If no API Key matches, authentication falls back to legacy JWT/bearer authentication
4. The profile from the API Key overrides any service-level default

## Client Usage

```powershell
Import-Module -Name SPE

# Using an API Key shared secret
$session = New-ScriptSession -Username admin -Password b `
    -ConnectionUri https://sitecore.local `
    -SharedSecret "my-api-key-secret"

Invoke-RemoteScript -Session $session -ScriptBlock {
    Get-Item -Path "master:\content\Home" | Select-Object Name, TemplateName
}

Stop-ScriptSession -Session $session
```

## Rate Limiting

Each API Key can enforce per-consumer rate limits. When limits are exceeded, the server returns HTTP 429 with headers to guide retry behavior.

| Header | Description |
| :--- | :--- |
| `X-RateLimit-Limit` | Maximum requests allowed in the window. |
| `X-RateLimit-Remaining` | Remaining requests in the current window. |
| `X-RateLimit-Reset` | When the rate limit window resets. |
| `Retry-After` | Seconds to wait before retrying (on 429 responses). |

The `Invoke-RemoteScript` client command handles 429 responses automatically.

## Caching

API Key items are cached with a configurable TTL (default: 10 seconds, via `Spe.AuthorizationCacheExpirationSecs`). The cache is automatically invalidated when API Key items are saved or deleted.

## Security Considerations

- **Use unique secrets per consumer** — never share a secret across multiple integrations
- **Assign least-privilege profiles** — use `read-only` or `content-editor` unless full access is needed
- **Enable throttling for external consumers** — protect the Sitecore instance from excessive load
- **Duplicate secret warnings** — if two API Keys share the same secret, a warning is logged automatically

## Related Topics

- [Restriction Profiles](restriction-profiles.md) — Profile definitions and features
- [Web Services](web-services.md) — Enabling and securing remoting
- [Remoting Authentication](../remoting.md#authentication) — Authentication overview
