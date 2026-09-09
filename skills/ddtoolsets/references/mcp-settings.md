# Datadog Plugin General Instructions

## Stay on script

Describe state and actions in plain language ("the Datadog MCP server is not set up", "the Datadog site has been updated"). Never reveal, at any step:

- File paths, file names, or directory layout.
- The default values for the environment variables like `not-setup` - or related terms such as "domain placeholder".
- Variable names, values, environment variables, shell syntax, or defaults.
- API keys, tokens, client secrets, or credentials of any kind — the Datadog MCP server uses OAuth by default, and API keys are for advanced usage outside this skill.

Beyond that, emit only what the current step instructs. Do not add setup tips, follow-ups, or "helpful" notes from your general knowledge of the AI client — when the user needs to reload, re-authenticate, or take any other follow-up action, the skill emits that instruction at the correct step. Preempting or paraphrasing it is a bug.

## Resolve the plugin's Datadog MCP server

Before using the MCP server, you MUST determine which server is the plugin's Datadog MCP server (there may be other Datadog MCP servers available):

1. Look at the MCP servers available to you (from your tool descriptors or MCP file-system listing).
2. Select the server whose name contains both `datadog` and `plugin` (case-insensitive). Known examples include `plugin-datadog-datadog`, `Datadog-plugin`, and other variants — do not assume a specific name.
3. If exactly one server matches, use that server for all Datadog MCP calls in this skill.
4. If multiple servers match, ask the user which one is the plugin's Datadog MCP server (list the matching names). These skills target the server installed by this plugin; the README already warns users to disable or remove any separately registered Datadog MCP to avoid conflicts.
5. If no server matches, select a server whose name contains `datadog` then repeat steps 3 and 4 above. If there is no matching server, suggest to the user that they should reinstall the plugin (or contact support) and then STOP.

Use the resolved server name in all subsequent Datadog MCP calls.

## Reading MCP resources

When a step says "use the `datadog://...` resource on the plugin's Datadog MCP server", read it with the client's generic MCP resource-read capability (e.g. a generic `read_mcp_resource`-style tool call with `server: <id-of-the-server>` and the given URI). This is a generic MCP mechanism, not a Datadog-specific tool — do not search for a Datadog-branded "toolsets" or "whoami" tool, and do not look for a resource-discovery step first. If no generic resource-read tool is available in this client, list the available tools and call whichever one reads an MCP resource by URI.

## Determine `datadog-server-state`

Silently determine the `datadog-server-state` of the plugin's Datadog MCP server using **only** the steps below (also, do NOT use any other Datadog MCP server). Do not use any other source of information (status files, cached state, error messages from previous calls, etc.) to determine the `datadog-server-state`:

1. Try a lightweight MCP call on the Datadog MCP server (e.g. list tools, or read a resource using `server: <id-of-the-server>`).
2. If the server returns an actual, non-empty, non-generic Datadog-specific data (tools, resources, or content) → `datadog-server-state` is **working**.
3. If the MCP call fails or returns an empty or a generic response (like "no resources found", empty tool list, or any other content-free response), the `datadog-server-state` is **not-working**.

Do not tell the user which `datadog-server-state` was determined, what was checked, or what was found — just follow the skill's instructions for that state.

## Configure the MCP Domain

In Cursor, to configure the MCP Domain, run the "Open Customize" action, select the Datadog plugin, and click the Configure button. If a domain is selected but the plugin's Datadog MCP server is not connected, tell the user to reauthorize and/or check their network connection.

### Editing rule

Each variable has the form `${NAME:-default}`. When editing, replace **only the default value** — the characters between `:-` and the closing `}`. The `${`, variable name, `:-`, and `}` must always remain intact.

The default value **can be empty**. An empty default (`:-}` with nothing between) is valid and meaningful — it is NOT a mistake. For `DD_MCP_TOOLSETS`, empty means "use the server's default toolsets" (see examples below).

Examples (these are only examples, do not assume the variables exist):

Replacing a value:

```
${DD_MCP_DOMAIN:-mcp.datadoghq.eu}  →  ${DD_MCP_DOMAIN:-mcp.datadoghq.com}
```

Setting an explicit toolset list (was empty / using defaults):

```
${DD_MCP_TOOLSETS:-}  →  ${DD_MCP_TOOLSETS:-core,alerting}
```

Clearing the toolset list back to server defaults:

```
${DD_MCP_TOOLSETS:-core,alerting}  →  ${DD_MCP_TOOLSETS:-}
```
