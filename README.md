> **This plugin is currently in Preview.**

# Datadog Cursor Plugin

Use Datadog directly in Cursor through a preconfigured Datadog MCP server. Query logs, metrics, traces, dashboards, monitors, notebooks, and more through natural conversation.

## What this plugin does

1. Registers the Datadog MCP server for Cursor through the plugin's root `mcp.json`.
2. Points Cursor at Datadog's managed remote MCP endpoint by default.
3. Lets you override the MCP endpoint with an environment variable when you need a different Datadog site.

Authentication is handled through Datadog's OAuth flow in clients that support remote authentication.

## Prerequisites

- [Datadog](https://www.datadoghq.com/) account with `MCP Read` and, if needed, `MCP Write`.
- [Cursor](https://cursor.com/) IDE with plugin support.

## Quick Start

> If you already have Datadog registered manually in `.cursor/mcp.json`, `~/.cursor/mcp.json`, or Cursor MCP settings, disable or remove that entry first. Running multiple Datadog MCP servers at once can cause duplicate tools and authentication confusion.

### Install the plugin

1. Open Cursor Settings.
2. Navigate to Plugins.
3. Install the `datadog` plugin.
4. Restart Cursor if prompted.

### Sign in

Once Cursor loads the Datadog MCP server, complete the OAuth sign-in flow in your browser if Cursor prompts for authentication.

### Ask Datadog questions

After the plugin is loaded, ask the agent to use Datadog tools:

```text
Show me logs from the last hour with error level
```

```text
What monitors are currently alerting?
```

```text
Find traces for service "api-gateway" with latency greater than 500ms
```

## Configuration

The plugin now loads MCP configuration from the repository root's `mcp.json`:

```json
{
  "mcpServers": {
    "datadog": {
      "url": "${env:DD_MCP_SERVER_URL:-https://mcp.us5.datadoghq.com/api/unstable/mcp-server/mcp}"
    }
  }
}
```

The config uses environment interpolation with a built-in default. If your Cursor build does not honor the `:-default` form, set `DD_MCP_SERVER_URL` explicitly before launching Cursor.

### Default endpoint

If you do nothing, the plugin uses this Datadog MCP endpoint:

```text
https://mcp.us5.datadoghq.com/api/unstable/mcp-server/mcp
```

### Override the endpoint

If your Datadog organization uses a different regional site, set `DD_MCP_SERVER_URL` before launching Cursor:

```bash
export DD_MCP_SERVER_URL="https://mcp.us3.datadoghq.com/api/unstable/mcp-server/mcp"
```

Restart Cursor after changing the environment variable so the new value is picked up.

## Troubleshooting

### The Datadog MCP server does not appear

- Make sure the plugin is installed.
- Restart Cursor after installing the plugin.
- Open Cursor MCP settings and confirm the `datadog` server is listed.

### The wrong Datadog site is used

Set `DD_MCP_SERVER_URL` to the MCP endpoint for the correct Datadog site, then restart Cursor.

### Authentication fails

- Confirm your Datadog user has `MCP Read` and, if needed, `MCP Write`.
- Retry the OAuth flow from Cursor.
- Remove any duplicate Datadog MCP entries from manual Cursor configuration.

### Something still is not working

- Restart Cursor.
- Check Cursor MCP logs for the `datadog` server.
- Compare your setup against the official Datadog MCP setup docs.

## Privacy & Security

- Connections are made directly to the configured Datadog MCP endpoint.
- Use environment variables for endpoint overrides instead of editing secrets into config files.
- Only install and enable Datadog MCP configurations you trust.

## Support

- [Datadog MCP Server Documentation](https://docs.datadoghq.com/bits_ai/mcp_server/)
- [Set Up the Datadog MCP Server](https://docs.datadoghq.com/bits_ai/mcp_server/setup/)
