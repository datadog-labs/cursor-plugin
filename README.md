> **This plugin is currently in Preview.**

# Datadog Cursor Plugin

Query Datadog directly from Cursor using natural language. Ask about logs, metrics, traces, dashboards, monitors, and more — just like asking a colleague.

## What you need

- A [Datadog](https://www.datadoghq.com/) account
- [Cursor](https://cursor.com/) IDE (v2.6.0+)

## Getting started

> If you already have the Datadog MCP server registered separately (e.g., in `.cursor/mcp.json`), disable or remove it first to avoid conflicts.

1. Open Cursor Settings by clicking on the gear icon in the left sidebar or by running the "Cursor Settings: Tools & MCP" command in the Command Palette.
2. Go to the **Plugins** section
3. Install the **datadog** plugin

Before using the MCP Server installed by the plugin, you’ll be prompted to select the Datadog domain. Then complete the OAuth sign-in and you’re good to go.

## Using the plugin

Once connected, just ask the agent anything about your Datadog data:

```
Show me error logs from the last hour
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

## Can't connect?

Run the `/datadog-mcp-setup` command in Cursor. It will walk you through configuring the Datadog MCP Server domain.

## Good to know

- Authentication is handled via OAuth in your browser.
- Your connection details are encrypted and stored in Cursor. No credentials are sent to the AI model provider.

## Support

- [Datadog MCP Server Documentation](https://docs.datadoghq.com/bits_ai/mcp_server/)
