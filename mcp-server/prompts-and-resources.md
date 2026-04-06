# Prompts & Resources

The MCP Server provides pre-built prompts that guide AI agents through common Sitecore workflows, and a resource for browsing the SPE script library.

## Prompts

MCP prompts are workflow templates that AI agents can invoke to perform multi-step operations with built-in best practices. Each prompt generates appropriate scripts, performs validation, and confirms destructive operations before execution.

### find_items

Find Sitecore items matching a natural language description. The agent selects the appropriate command based on the query — `Get-Item` for specific paths, `Get-ChildItem -Recurse` for scoped traversals, or `Find-Item -Criteria` for large-scale index searches.

| Parameter | Type | Description |
| :--- | :--- | :--- |
| `query` | string | Natural language description (e.g., "all pages modified in the last week"). |

**Example usage:**

> "Find all items using the Article template under /sitecore/content"

### content_audit

Audit a section of the Sitecore content tree for common issues.

| Parameter | Type | Description |
| :--- | :--- | :--- |
| `path` | string | Content path to audit (e.g., `/sitecore/content/Home`). |
| `checks` | string | What to check: `missing-fields`, `broken-links`, `unpublished`, `stale`, or `all` (default: `all`). |

The audit runs appropriate checks using `Get-ChildItem`, `Get-ItemReference`, and date comparisons, then returns a structured report grouped by check type.

**Example usage:**

> "Audit /sitecore/content/Home for broken links and stale content"

### manage_security

Manage Sitecore users, roles, and security with guided confirmation steps.

| Parameter | Type | Description |
| :--- | :--- | :--- |
| `operation` | string | One of: `create-user`, `modify-user`, `create-role`, `audit-permissions`, `list-users`. |
| `details` | string | Details about the operation (e.g., username, role name, or audit path). |

The workflow performs read-only checks first, explains planned changes, waits for confirmation, executes write operations, and verifies the new state.

**Example usage:**

> "Create a new user sitecore\content-editor with the Author role"

### publish_content

Publish Sitecore content with pre-publish validation and post-publish verification.

| Parameter | Type | Description |
| :--- | :--- | :--- |
| `path` | string | Content path to publish (e.g., `/sitecore/content/Home`). |
| `mode` | string | Publish mode: `smart` (default), `full`, or `incremental`. |
| `targets` | string | Publishing targets, comma-separated (default: all configured targets). |

The workflow validates command availability, performs pre-publish checks, executes `Publish-Item`, and verifies post-publish state.

**Example usage:**

> "Smart publish /sitecore/content/Home and its children"

### run_script_library

Execute a script from the SPE script library with explanation and confirmation.

| Parameter | Type | Description |
| :--- | :--- | :--- |
| `scriptPath` | string | Path to the script in the SPE Script Library (e.g., `Content Maintenance/Clean Up`). |

The workflow fetches the script via the `spe://scripts/` resource, explains what it does, highlights any risks, waits for confirmation, executes the script, and shows results.

**Example usage:**

> "Run the Content Maintenance/Clean Up script from the library"

## Resources

MCP resources expose data that AI agents can read on demand.

### SPE Script Library

**URI Template:** `spe://scripts/{path}`

Browse the SPE script library tree or retrieve a specific script's content.

| Path | Returns |
| :--- | :--- |
| Directory path | Listing of child items with Name, ItemPath, and TemplateName. |
| Script path | The script's name, path, and full script content. |

**Example:**

- `spe://scripts/` — Lists top-level script library folders
- `spe://scripts/Content Maintenance` — Lists scripts in the Content Maintenance folder
- `spe://scripts/Content Maintenance/Clean Up` — Returns the Clean Up script content

The `run_script_library` prompt uses this resource to fetch scripts before execution.
