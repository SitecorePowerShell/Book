# Tools Reference

The MCP Server exposes 18 tools organized by category. Each tool is callable by AI agents through the MCP protocol.

{% hint style="warning" %}
**Authentication:** Use [API key authentication](../remoting.md#authentication) with a dedicated `sitecore\mcp` user for impersonation rather than `sitecore\admin`. See [Security](security.md) for detailed guidance.
{% endhint %}

## Connection

### spe_test_connection

Test connectivity to the Sitecore instance. Validates authentication and returns version information, current user, and detected PowerShell language mode. Provides diagnostic hints on failure.

**Parameters:** None

### spe_context

Get key Sitecore instance information: version, databases, sites, and configured languages.

**Parameters:** None

## Script Execution

### spe_execute

Execute a PowerShell script on the Sitecore instance via SPE remoting.

| Parameter | Type | Required | Description |
| :--- | :--- | :--- | :--- |
| `script` | string | Yes | The PowerShell script to execute. |
| `sessionId` | string | No | Session ID for persistent sessions. |
| `outputFormat` | string | No | Output format: `json` (default) or `raw`. |
| `streamProgress` | boolean | No | Enable progress notifications for long-running scripts. |
| `pollInterval` | integer | No | Polling interval in seconds when streaming (default: 1). |

## Sessions

Persistent sessions allow multi-step stateful operations where variables and state are preserved between calls.

### spe_session_open

Open a persistent PowerShell session.

| Parameter | Type | Required | Description |
| :--- | :--- | :--- | :--- |
| `id` | string | No | Custom session ID. Auto-generated if omitted. |

### spe_session_close

Close a persistent session and free server resources.

| Parameter | Type | Required | Description |
| :--- | :--- | :--- | :--- |
| `sessionId` | string | Yes | The session ID to close. |

### spe_list_sessions

List all active persistent sessions opened in this server instance. Returns session IDs, instance, creation time, last used time, and age.

**Parameters:** None

## Command Discovery

### spe_list_commands

List available SPE commands, filtered by the active security policy. Only commands allowed by the current security mode are returned.

**Parameters:** None

### spe_get_command_help

Get full help documentation for a specific SPE command including parameters, examples, and detailed description.

| Parameter | Type | Required | Description |
| :--- | :--- | :--- | :--- |
| `name` | string | Yes | The command name (e.g., `Get-Item`, `Find-Item`). |

## Instances

### spe_list_instances

List all configured Sitecore instances and show which one is currently active.

**Parameters:** None

### spe_switch_instance

Switch the active Sitecore instance for subsequent tool calls.

| Parameter | Type | Required | Description |
| :--- | :--- | :--- | :--- |
| `instanceName` | string | Yes | Name of the instance to switch to (e.g., `staging`, `production`). |

## Files and Media

### spe_upload_file

Upload a file to the Sitecore server filesystem.

| Parameter | Type | Required | Description |
| :--- | :--- | :--- | :--- |
| `fileContent` | string | Yes | Base64-encoded file content. |
| `fileName` | string | Yes | File name with extension (e.g., `script.ps1`). |
| `destination` | string | Yes | Server filesystem destination path (e.g., `/App_Data/scripts`). |

### spe_upload_media

Upload a file to the Sitecore media library.

| Parameter | Type | Required | Description |
| :--- | :--- | :--- | :--- |
| `fileContent` | string | Yes | Base64-encoded file content. |
| `fileName` | string | Yes | File name with extension (e.g., `logo.png`). |
| `destination` | string | Yes | Media library destination path (e.g., `/sitecore/media library/Images`). |
| `database` | string | No | Target database (default: `master`). |

### spe_download_media

Download a media item from the Sitecore media library as base64-encoded data.

| Parameter | Type | Required | Description |
| :--- | :--- | :--- | :--- |
| `mediaPath` | string | Yes | Media library path (e.g., `/sitecore/media library/Images/logo`). |
| `database` | string | No | Target database (default: `master`). |

## Workflows

### spe_get_workflows

List all workflows defined in Sitecore with their states and commands.

**Parameters:** None

### spe_get_workflow_state

Get the current workflow state of a Sitecore item.

| Parameter | Type | Required | Description |
| :--- | :--- | :--- | :--- |
| `itemPath` | string | Yes | Sitecore item path (e.g., `/sitecore/content/Home`). |
| `database` | string | No | Database name (default: `master`). |

### spe_advance_workflow

Execute a workflow command to advance a Sitecore item to the next workflow state.

| Parameter | Type | Required | Description |
| :--- | :--- | :--- | :--- |
| `itemPath` | string | Yes | Sitecore item path. |
| `commandId` | string | Yes | Workflow command ID to execute. |
| `comment` | string | No | Comment for the workflow transition. |
| `database` | string | No | Database name (default: `master`). |

## Script Generation

### spe_script_templates

Get pre-built, vetted PowerShell script templates for common Sitecore tasks.

| Parameter | Type | Required | Description |
| :--- | :--- | :--- | :--- |
| `category` | string | No | Template category (e.g., `content-query`, `publishing`, `security`, `reporting`, `maintenance`). Omit to list all categories. |

### spe_generate_report

Returns generation rules and examples for creating Sitecore reports. Supports execution mode (direct script output) and artifact mode (downloadable files).

| Parameter | Type | Required | Description |
| :--- | :--- | :--- | :--- |
| `query` | string | Yes | Natural language description of the report (e.g., `locked items`, `stale content older than 90 days`). |
| `rootPath` | string | No | Root content path (default: `/sitecore/content`). |
