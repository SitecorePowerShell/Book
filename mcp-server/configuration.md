# Configuration

The MCP Server is configured through `appsettings.json` or environment variables. All settings live under the `Spe` section.

## Environment Variables

Environment variables use the `SPE_` prefix and override `appsettings.json` values. This is the recommended approach for sensitive values like secrets.

| Variable | Description |
| :--- | :--- |
| `SPE_URL` | Sitecore instance URL. |
| `SPE_SHARED_SECRET` | Authentication shared secret or API key. |
| `SPE_USERNAME` | User to impersonate (e.g., `sitecore\mcp`). |
| `SPE_TRANSPORT` | Transport protocol: `stdio` or `http`. |

Within `appsettings.json`, use the `env:` prefix to reference environment variables:

```json
{
  "Spe": {
    "Instances": {
      "default": {
        "Auth": {
          "SharedSecret": "env:MY_SECRET_VAR"
        }
      }
    }
  }
}
```

## Transport

| Setting | Default | Description |
| :--- | :--- | :--- |
| `Transport` | `stdio` | Transport protocol: `stdio` or `http`. |
| `HttpPort` | `3001` | Port for HTTP transport. |

```json
{
  "Spe": {
    "Transport": "stdio",
    "HttpPort": 3001
  }
}
```

Use `stdio` for Claude Desktop and IDE integrations. Use `http` for web-based clients or shared environments.

## Instances

Configure one or more Sitecore instances. The MCP Server can switch between instances at runtime using the `spe_switch_instance` tool.

```json
{
  "Spe": {
    "Instances": {
      "default": {
        "Url": "https://sitecore.local",
        "Auth": {
          "SharedSecret": "env:SPE_SHARED_SECRET",
          "Username": "sitecore\\mcp"
        },
        "DefaultDatabase": "master"
      },
      "staging": {
        "Url": "https://staging.example.com",
        "Auth": {
          "SharedSecret": "env:SPE_STAGING_SECRET",
          "Username": "sitecore\\mcp"
        },
        "DefaultDatabase": "master"
      }
    }
  }
}
```

| Setting | Description |
| :--- | :--- |
| `Url` | Sitecore instance URL. |
| `Auth.SharedSecret` | Shared secret or API key for authentication. |
| `Auth.Username` | User to impersonate for the remote session. |
| `DefaultDatabase` | Default Sitecore database (typically `master`). |

## Security

See the [Security & Sandbox](security.md) page for detailed explanations of each mode.

```json
{
  "Spe": {
    "Security": {
      "Mode": "filtered",
      "AllowedCommandPrefixes": ["Get-", "Find-", "Search-"],
      "BlockedCommands": ["Remove-Item", "Set-ItemAcl", "Invoke-Expression"],
      "ObfuscationChecks": true,
      "ConfirmationRequired": ["Publish-Item", "Set-Item"]
    }
  }
}
```

| Setting | Default | Description |
| :--- | :--- | :--- |
| `Mode` | `filtered` | Sandbox mode: `read-only`, `filtered`, or `unrestricted`. |
| `AllowedCommandPrefixes` | `[]` | Command prefixes allowed in `read-only` mode. |
| `BlockedCommands` | `[]` | Commands blocked in `filtered` mode. |
| `ObfuscationChecks` | `true` | Enable AST-based obfuscation detection. |
| `ConfirmationRequired` | `[]` | Commands that require explicit confirmation. |

## Rate Limits

Protect the Sitecore instance from excessive load.

```json
{
  "Spe": {
    "RateLimits": {
      "MaxConcurrentExecutions": 3,
      "MaxExecutionsPerMinute": 30,
      "MaxScriptSizeBytes": 51200
    }
  }
}
```

| Setting | Default | Description |
| :--- | :--- | :--- |
| `MaxConcurrentExecutions` | `3` | Maximum scripts running simultaneously. |
| `MaxExecutionsPerMinute` | `30` | Maximum script executions per minute. |
| `MaxScriptSizeBytes` | `51200` | Maximum script size in bytes (50 KB). |

## Resilience

Built-in retry and circuit breaker powered by Polly.

```json
{
  "Spe": {
    "Resilience": {
      "MaxRetryAttempts": 3,
      "CircuitBreakerThreshold": 5,
      "TimeoutSeconds": 120
    }
  }
}
```

| Setting | Default | Description |
| :--- | :--- | :--- |
| `MaxRetryAttempts` | `3` | Number of retry attempts on transient failures. |
| `CircuitBreakerThreshold` | `5` | Consecutive failures before the circuit opens. |
| `TimeoutSeconds` | `120` | Maximum time for a single script execution. |

## Audit

Log all script executions for compliance and troubleshooting.

```json
{
  "Spe": {
    "Audit": {
      "Enabled": true,
      "LogDirectory": "./logs/audit",
      "RetentionDays": 30
    }
  }
}
```

| Setting | Default | Description |
| :--- | :--- | :--- |
| `Enabled` | `true` | Enable audit logging. |
| `LogDirectory` | `./logs/audit` | Directory for audit log files. |
| `RetentionDays` | `30` | Number of days to retain audit logs. |

## Media

Limits for media upload and download operations.

```json
{
  "Spe": {
    "Media": {
      "MaxUploadSizeBytes": 10485760,
      "MaxDownloadSizeBytes": 10485760
    }
  }
}
```

| Setting | Default | Description |
| :--- | :--- | :--- |
| `MaxUploadSizeBytes` | `10485760` | Maximum upload size (10 MB). |
| `MaxDownloadSizeBytes` | `10485760` | Maximum download size (10 MB). |

## Complete Example

```json
{
  "Spe": {
    "Transport": "stdio",
    "HttpPort": 3001,
    "Instances": {
      "default": {
        "Url": "https://sitecore.local",
        "Auth": {
          "SharedSecret": "env:SPE_SHARED_SECRET",
          "Username": "sitecore\\mcp"
        },
        "DefaultDatabase": "master"
      }
    },
    "Security": {
      "Mode": "filtered",
      "BlockedCommands": ["Remove-Item", "Set-ItemAcl"],
      "ObfuscationChecks": true,
      "ConfirmationRequired": ["Publish-Item"]
    },
    "RateLimits": {
      "MaxConcurrentExecutions": 3,
      "MaxExecutionsPerMinute": 30,
      "MaxScriptSizeBytes": 51200
    },
    "Resilience": {
      "MaxRetryAttempts": 3,
      "CircuitBreakerThreshold": 5,
      "TimeoutSeconds": 120
    },
    "Audit": {
      "Enabled": true,
      "LogDirectory": "./logs/audit",
      "RetentionDays": 30
    },
    "Media": {
      "MaxUploadSizeBytes": 10485760,
      "MaxDownloadSizeBytes": 10485760
    }
  }
}
```
