# Security & Sandbox

The MCP Server includes a security layer that validates scripts before they are sent to SPE for execution. This is a client-side safety net that operates **in addition to** SPE's own server-side security model.

{% hint style="warning" %}
**Remoting only.** The sandbox modes described here apply exclusively to scripts executed through the MCP Server's remoting connection. They do not affect other SPE integration points such as the ISE, Console, or Content Editor scripts, which are governed by SPE's own [security configuration](../security/).
{% endhint %}

## Why Sandbox the MCP Server?

When an AI agent generates and executes PowerShell scripts, there is an inherent risk that the generated code may:

- Perform unintended destructive operations (deleting content, modifying security)
- Access sensitive data or configuration
- Execute obfuscated or injected commands
- Exceed intended scope of operations

The sandbox provides defense-in-depth by validating scripts **before** they reach the Sitecore instance, complementing SPE's server-side [restriction profiles](../remoting.md#authentication) and Constrained Language Mode.

## Sandbox Modes

The MCP Server supports three sandbox modes, configured via the `Security.Mode` setting.

### read-only

The most restrictive mode. Uses an **allowlist** — only explicitly permitted command prefixes are allowed.

| Setting | Description |
| :--- | :--- |
| `AllowedCommandPrefixes` | List of permitted command verb prefixes (e.g., `Get-`, `Find-`, `Search-`). |

Scripts containing any command not matching an allowed prefix are rejected before execution. This mode is ideal for reporting and read-only integrations where the agent should never modify content.

### filtered

A balanced mode. Uses a **blocklist** — known dangerous commands are blocked, and obfuscation detection is enabled.

| Setting | Description |
| :--- | :--- |
| `BlockedCommands` | List of blocked commands (e.g., `Remove-Item`, `Set-ItemAcl`). |
| `ObfuscationChecks` | Enables AST-based detection of obfuscation patterns. |
| `ConfirmationRequired` | Commands that require explicit confirmation before execution. |

This mode allows general scripting while blocking destructive operations. It is suitable for content management scenarios where the agent needs write access to specific areas.

### unrestricted

No client-side script validation. Scripts are passed directly to SPE for execution. Server-side security (restriction profiles, CLM) still applies.

Use this mode only when:
- The SPE instance has robust server-side restriction profiles configured
- The service account has minimal Sitecore permissions
- The environment is isolated (e.g., local development)

## AST-Based Script Validation

In `read-only` and `filtered` modes, the MCP Server performs static analysis on scripts using PowerShell's Abstract Syntax Tree (AST) before execution. This catches threats that simple string matching would miss.

### Obfuscation Detection

The following patterns are detected and blocked:

| Pattern | Description |
| :--- | :--- |
| `Invoke-Expression` | Dynamic script execution that could bypass command checks. |
| .NET type access | Direct access to .NET types that could circumvent restrictions. |
| Base64 encoding | Encoded payloads that could hide malicious content. |
| Dynamic invocation | Variable-based command execution (e.g., `& $variable`). |
| Backtick obfuscation | Excessive use of backtick escaping to disguise commands. |

## Decision Matrix

| Scenario | Recommended Mode | Rationale |
| :--- | :--- | :--- |
| Reporting and analytics | `read-only` | Agent only needs to query data. |
| Content management | `filtered` | Agent needs controlled write access. |
| Development / local | `unrestricted` | Full access in isolated environment. |
| Production CM | `filtered` or `read-only` | Minimize risk on shared environments. |

## Configuration Example

```json
{
  "Spe": {
    "Security": {
      "Mode": "filtered",
      "BlockedCommands": [
        "Remove-Item",
        "Remove-ItemVersion",
        "Set-ItemAcl",
        "Invoke-Expression"
      ],
      "ObfuscationChecks": true,
      "ConfirmationRequired": [
        "Publish-Item",
        "Set-Item"
      ],
      "AllowedCommandPrefixes": []
    }
  }
}
```

## Defense in Depth

The MCP Server's sandbox is one layer in a multi-layer security model:

| Layer | Component | Scope |
| :--- | :--- | :--- |
| **Client-side** | MCP Server sandbox | Script validation before transmission |
| **Transport** | HTTPS | Encrypted communication |
| **Server-side** | SPE restriction profiles | Command and path restrictions per API key |
| **Server-side** | Constrained Language Mode | PowerShell language restrictions |
| **Server-side** | Sitecore permissions | Item-level access control |

Each layer operates independently. Even if one layer is bypassed, the others provide protection.

## Best Practices

- **Start with `read-only`** and relax to `filtered` only when needed
- **Create a dedicated service account** (`sitecore\mcp`) with minimal Sitecore permissions
- **Use API keys** with per-key restriction profiles rather than a shared secret
- **Enable obfuscation checks** in `filtered` mode
- **Review audit logs** regularly — see [Configuration](configuration.md) for audit settings
- **Never expose the MCP Server** on the public internet without additional network-level security

## Related Topics

- [SPE Security](../security/) — Server-side security configuration
- [Web Services Security](../security/web-services.md) — Enabling and securing remoting
- [Remoting Authentication](../remoting.md#authentication) — API keys and restriction profiles
