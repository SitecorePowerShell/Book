# Item Path Restrictions

{% hint style="info" %}
Introduced in SPE 9.0.
{% endhint %}

Item path restrictions control which Sitecore content paths are accessible through remoting endpoints. They are configured per [restriction profile](restriction-profiles.md) and use prefix-based matching to block or allow access to entire content subtrees.

## Modes

| Mode | Behavior |
| :--- | :--- |
| **Blocklist** | All paths accessible except those explicitly blocked. |
| **Allowlist** | All paths blocked except those explicitly allowed. |

## Configuration

Path restrictions are defined within a profile's `<itemPathRestrictions>` section in `Spe.config`:

### Blocklist Example

```xml
<profile name="read-only" languageMode="ConstrainedLanguage" commandMode="blocklist">
  <itemPathRestrictions mode="blocklist">
    <blockedPaths>
      <path>/sitecore/system/Modules/PowerShell/Settings/Remoting</path>
    </blockedPaths>
  </itemPathRestrictions>
</profile>
```

### Allowlist Example

```xml
<profile name="content-editor" languageMode="ConstrainedLanguage" commandMode="allowlist">
  <itemPathRestrictions mode="allowlist">
    <allowedPaths>
      <path>/sitecore/content</path>
      <path>/sitecore/media library</path>
      <path>/sitecore/layout</path>
    </allowedPaths>
  </itemPathRestrictions>
</profile>
```

## Default Restrictions

| Profile | Mode | Paths |
| :--- | :--- | :--- |
| `unrestricted` | None | No restrictions |
| `read-only` | Blocklist | Blocks `/sitecore/system/Modules/PowerShell/Settings/Remoting` |
| `read-only-strict` | Blocklist | Blocks `/sitecore/system/Modules/PowerShell/Settings/Remoting` |
| `content-editor` | Allowlist | Allows `/sitecore/content`, `/sitecore/media library`, `/sitecore/layout` |

## Prefix Matching

Paths use prefix matching — blocking `/sitecore/system/Modules/PowerShell/Settings/Remoting` also blocks all children under that path. This prevents remote consumers from enumerating API Keys, restriction profiles, or trusted script configurations.

## Item-Based Overrides

Restriction Profile override items can add paths via **Treelist** fields:

- `AdditionalBlockedPaths` — add paths to the blocklist
- `AdditionalAllowedPaths` — add paths to the allowlist

Since Treelist stores item GUIDs, restrictions survive item renames and moves. GUIDs are resolved to paths at merge time.

Overrides are **additive** — they can add restrictions but cannot remove base profile restrictions.

## Enforcement

Path restrictions are enforced in the Sitecore provider (`PsSitecoreItemProvider`) at the item access layer:

| Behavior | Description |
| :--- | :--- |
| `Get-Item` on a blocked path | Returns a non-terminating error with the profile name in the message. |
| `Get-ChildItem` under a partially blocked path | Blocked children are silently filtered from results. |
| Access by item ID | The item's path is resolved and checked before access is allowed. |
| `Enforce` mode | Blocks access and returns an error. |
| `Audit` mode | Logs the violation but allows access (dry-run). |

## Audit Logging

Violations are logged according to the profile's audit level:

```
[Security] action=pathBlocked profile=read-only path=/sitecore/system/Modules/PowerShell/Settings/Remoting/API Keys user=sitecore\mcp
```

## Use Cases

- **Protect security configuration** — block access to `/sitecore/system/Modules/PowerShell/Settings/Remoting` to prevent enumeration of API Keys and profiles
- **Scope content editors** — restrict to `/sitecore/content` and `/sitecore/media library` only
- **Isolate tenants** — in multi-tenant installations, restrict each consumer to their own content subtree

{% hint style="info" %}
Item path restrictions operate at the provider level. Direct .NET API calls like `item.Children` from within a script are not restricted. This is acceptable because ConstrainedLanguage mode already prevents arbitrary method invocation in constrained profiles.
{% endhint %}

## Related Topics

- [Restriction Profiles](restriction-profiles.md) — Profile configuration and features
- [CLM Migration Guide](clm-migration.md) — Rollout steps including path restriction planning
