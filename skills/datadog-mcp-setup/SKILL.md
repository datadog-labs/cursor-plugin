---
name: datadog-mcp-setup
description: Guide for setting up and connecting to the Datadog-managed MCP server from Cursor using the plugin's remote `mcp.json` configuration. Use when configuring Datadog MCP, overriding the default server URL, or troubleshooting Datadog MCP setup.
---

# Datadog MCP Server Setup

This skill guides you through connecting Cursor to the Datadog-managed MCP server so the agent can query Datadog telemetry and platform features directly from chat.

## Setup

### 1. Install the plugin

Install the `datadog` plugin in Cursor. The plugin ships with an `mcp.json` that registers the Datadog MCP server automatically.

### 2. Use the default Datadog MCP URL or override it

By default, the plugin points Cursor at this Datadog MCP endpoint:

```text
https://mcp.us5.datadoghq.com/api/unstable/mcp-server/mcp
```

If you need a different Datadog site, set `DD_MCP_SERVER_URL` before launching Cursor.

Example:

```sh
export DD_MCP_SERVER_URL="https://mcp.us3.datadoghq.com/api/unstable/mcp-server/mcp"
```

### 3. Review the plugin `mcp.json`

The root `mcp.json` is configured to read from `DD_MCP_SERVER_URL` and fall back to the default US5 endpoint:

```json
{
  "mcpServers": {
    "datadog": {
      "url": "${env:DD_MCP_SERVER_URL:-https://mcp.us5.datadoghq.com/api/unstable/mcp-server/mcp}"
    }
  }
}
```

If your Cursor build does not support the `:-default` interpolation form, set `DD_MCP_SERVER_URL` explicitly before launching Cursor.

### 4. Authenticate

Datadog MCP uses OAuth 2.0 in clients that support remote authentication. After the plugin is installed and Cursor loads the MCP server, sign in through the browser prompt from Cursor.

You need Datadog permissions that allow MCP access:

- `MCP Read` for read tools
- `MCP Write` for write tools

### 5. Restart or reload Cursor

After installing the plugin or changing `DD_MCP_SERVER_URL`, restart Cursor or reload the window so Cursor picks up the updated MCP configuration.

## Troubleshooting

### The Datadog MCP server does not appear

- Confirm the plugin is installed.
- Open Cursor MCP settings and verify the `datadog` server is listed.
- Restart Cursor after installing the plugin or changing environment variables.

### The wrong Datadog site opens

Set `DD_MCP_SERVER_URL` to the MCP endpoint for the correct Datadog site, then restart Cursor.

### Authentication fails

- Confirm your Datadog user has the required `MCP Read` or `MCP Write` permissions.
- Retry the OAuth sign-in flow from Cursor.
- If you previously registered another Datadog MCP server manually, remove it to avoid conflicts.

## References

- [Datadog MCP Server setup docs](https://docs.datadoghq.com/bits_ai/mcp_server/setup/)
- [Datadog MCP Server overview](https://docs.datadoghq.com/bits_ai/mcp_server/)
