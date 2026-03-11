> **This plugin is currently in Preview.**

# Datadog Cursor Plugin

Use Datadog directly in Cursor. Query logs, metrics, traces, dashboards, and more through natural conversation. Configure your domain and organization, with automatic OAuth authentication.

## What this plugin does

The Cursor Plugin connects Cursor to [Datadog](https://docs.datadoghq.com/bits_ai/mcp_server/) automatically — no manual registration needed.


Authentication is handled via OAuth in your browser. Configuration changes are applied without restarting Cursor.

Through the Datadog MCP server, you can query logs, metrics, traces, dashboards, monitors, notebooks, and more.

## Prerequisites

- [Datadog](https://www.datadoghq.com/) account with the `MCP Read` and `MCP Write` permissions.
- [Cursor](https://cursor.com/) IDE (v2.6.0+)

## Quick Start

> If you have the Datadog MCP server registered directly (e.g., in `.cursor/mcp.json` or via MCP settings), you *must* disable or remove it first. This plugin provides the MCP server — having both active causes duplicate tools and conflicts.

### Install the Plugin

Add the marketplace and install the plugin from Cursor Settings → Plugins:

1. Open Cursor Settings (Cmd+, or Ctrl+,)
2. Navigate to the Plugins section
3. Install the `datadog` plugin
4. You may need to restart Cursor after installing.

The plugin expects MCP config like the repo’s [mcp.json](https://github.com/datadog-labs/cursor-plugin/blob/main/mcp.json), which uses the `DD_MCP_DOMAIN` environment variable for the MCP hostname (e.g. `mcp.datadoghq.com` for us1, `mcp.us3.datadoghq.com` for us3). The same config works for all regions when `DD_MCP_DOMAIN` is set.

A skill is provided to help you pick the correct domain for your datacenter — run the `datadog-mcp-setup` skill to set `DD_MCP_DOMAIN` so you don’t have to edit URLs by hand. The MCP endpoint is controlled by `DD_MCP_DOMAIN` (domain only, not full URL); default is `mcp.datadoghq.com` (us1). For your region, see the table below.

| Region | MCP domain (`DD_MCP_DOMAIN`)   |
|--------|--------------------------------|
| us1    | mcp.datadoghq.com              |
| us3    | mcp.us3.datadoghq.com          |
| us5    | mcp.us5.datadoghq.com          |
| eu1    | mcp.eu1.datadoghq.com          |
| ap1    | mcp.ap1.datadoghq.com          |
| ap2    | mcp.ap2.datadoghq.com          |



## Using the Plugin

### Ask Datadog Questions

Once configured, ask the agent to interact with Datadog:

```
Show me logs from the last hour with error level
```

```
What monitors are currently alerting?
```

```
Find traces for service "api-gateway" with latency > 500ms
```

```
List my dashboards
```

```
Create a notebook analyzing database performance
```

### Changing Domain

By default, the plugin connects to `app.datadoghq.com` (US1). If your organization uses a different Datadog site (US3, EU1, etc.) or a custom domain, change it via `/ddconfig` and select "Domain".

### Switching Organizations

If you have access to multiple Datadog organizations, you can switch between them using the switch organization option of `/ddconfig` - the plugin automatically handles authentication and reloads.

### Manage Toolsets

The plugin exposes Datadog tools through configurable toolsets. By default, the **core** toolset is enabled (logs, metrics, traces). Use `/ddconfig` → Toolsets to enable or disable additional ones.

> **Note:** Cursor enforces a 40-tool limit across all MCP servers. If you have many tools from other MCP servers, you may need to disable some toolsets to stay within the limit.

### Tip

After running the `/ddconfig` command for the first time, you may interact with the agent to make configuration changes:

```
Enable the Datadog synthetics toolset
```

## Configuration Behavior

The plugin remembers its settings separately for each folder where you run Cursor.

- If you start multiple instances of Cursor from the same folder, all instances share the same Datadog configuration (domain, organization, and enabled toolsets).
- If you start Cursor from a different folder, the plugin begins with your most recent Datadog setup so you don’t need to go through OAuth again.

After the first launch in a new folder, that folder keeps its own independent configuration. Changes made there will not affect other folders.

## Known Limitations

### Tool Limit

Cursor has a 40-tool hard limit across all MCP servers. If you enable many Datadog toolsets alongside other MCP servers, you may exceed this limit. Use `/ddconfig` → Toolsets to manage which toolsets are active.

### Reset

Reset clears all config for the plugin across all sessions. There is currently no way to reset an individual session.

### OAuth Only

The plugin authenticates through OAuth. API and Application key authentication is not currently supported.

### No Browser

At the moment, the plugin requires opening a browser on the machine where it is running, so it does not support remote or headless environments.

## Troubleshooting

### Uninstall the Plugin

To remove the plugin, remove the plugin from Cursor Settings → Plugins, then restart Cursor.

### Wrong Datadog domain on first connection

The plugin defaults to `app.datadoghq.com` (US1). If you see the wrong sign-in page or authentication fails because your organization is on a different site, run the `datadog-mcp-setup` skill to set the correct domain, or set `DD_MCP_DOMAIN` (e.g. `mcp.us3.datadoghq.com`) manually. 

### Multiple browser tabs open during sign-in

If the plugin is installed globally and you have several Cursor windows open, each one runs its own plugin instance. On first install (or after a plugin reset), they all try to authenticate at the same time, opening multiple browser tabs that are hard to tell apart. Installing the plugin for a specific project may avoid this.

If this happens, close all Cursor windows except one, restart it, and sign in. Once authentication succeeds, reopen the other windows — they'll pick up the saved authentication automatically.

### Not signed-in to Datadog

The plugin opens your browser to sign in to Datadog when Cursor starts. If you missed it or the session expired, go to `Tools & MCP` in Cursor Settings and re-authenticate.

### OAuth callback port conflict

The plugin tries several localhost ports for the OAuth callback (`40100`, `40101`, `50200`, `50201`, `51234`, `52345`, `53456`, `54567`, `8080`, `8081`). If some of those ports are unavailable set the `DD_OAUTH_PORT` environment variable using one of the valid ports:

```bash
DD_OAUTH_PORT=8080
```


## Privacy & Security

- Connection details are encrypted and stored locally on your machine
- No credentials are sent to the AI model provider
- Connections are made directly to your Datadog instance

### Data Collection

Datadog collects certain information about your usage of this plugin, including how you interact with it, whether errors occurred while using it, what caused those errors, and anonymous usage identifiers in accordance with the [Datadog Privacy Policy][datadog_privacy_policy]. This data is used to help improve the plugin's performance and features. No file paths, source code, tokens, or secrets are ever included.

To disable data collection, set the following environment variable before starting Cursor:

```bash
export DD_PLUGIN_TELEMETRY_OFF=true
```

[datadog_privacy_policy]: https://www.datadoghq.com/legal/privacy/

## MCP Server

This plugin provides the Datadog MCP Server which connects Cursor to your Datadog account.

## Support

For issues or questions:

- [Datadog MCP Server Documentation](https://docs.datadoghq.com/bits_ai/mcp_server/)
- Post on #datadog-ai-for-developers in the [Datadog Public Slack](https://chat.datadoghq.com/)
