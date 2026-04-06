# Trusted Scripts

{% hint style="info" %}
Introduced in SPE 9.0.
{% endhint %}

When a [restriction profile](restriction-profiles.md) enables Constrained Language Mode (CLM), scripts are prevented from using .NET types, dynamic method calls, and other advanced PowerShell features. The Trusted Scripts registry allows specific scripts to bypass these restrictions when they legitimately need full language access.

## Overview

Trust is **binary** — a script is either trusted or untrusted. There are no intermediate trust levels.

Trusted scripts are managed as Sitecore content items at:

```
/sitecore/system/Modules/PowerShell/Settings/Remoting/Trusted Scripts/
```

Each trust item uses the `Trusted Script` template and references one or more scripts via a Treelist field. Nested folders are supported for organization.

## Trust Item Fields

| Field | Type | Description |
| :--- | :--- | :--- |
| `Enabled` | Checkbox | Activate or deactivate this trust entry. |
| `Script` | Treelist | References to script items in the SPE Script Library. A single trust item can cover multiple scripts. |
| `AllowedProfiles` | Single-Line Text | Comma-separated profile names this trust applies to. Empty means trusted under all profiles. |

## Profile-Bound Trust

Trust can be limited to specific restriction profiles:

- A script trusted for `read-only` will **not** be trusted under `read-only-strict` unless explicitly listed
- Empty `AllowedProfiles` means the script is trusted under **all** profiles
- This allows fine-grained control over which scripts bypass CLM in which contexts

## Built-in Trusted Scripts

SPE ships with pre-registered trusted scripts under `Trusted Scripts/SPE/`:

- `Core/Platform/` — core platform functions required for SPE operation
- `Training/Web API/` — training and web API examples

These ensure SPE's own functionality works correctly under constrained profiles.

## Managing Trusted Scripts

### Adding a Trusted Script

1. Navigate to `/sitecore/system/Modules/PowerShell/Settings/Remoting/Trusted Scripts/`
2. Create a folder to organize your trust entries (optional)
3. Create a new item using the `Trusted Script` template
4. Use the Treelist field to select the script items that should be trusted
5. Optionally set `AllowedProfiles` to limit which profiles honor this trust
6. Check `Enabled` to activate

### How Trust is Evaluated

When a remoting request executes a script:

1. The script's item ID is looked up in the trust registry
2. If found and enabled, the `AllowedProfiles` are checked against the active profile
3. If trusted, the script runs with full language access regardless of the profile's language mode
4. If untrusted, the script runs under the profile's language mode constraints

## Caching

Trust lookups use O(1) item ID-based lookup for performance. The cache is automatically invalidated when trust items are saved or deleted via `TrustedScriptSaveHandler`.

## Related Topics

- [Restriction Profiles](restriction-profiles.md) — Profiles that enable CLM
- [CLM Migration Guide](clm-migration.md) — How to identify and register trusted scripts during rollout
