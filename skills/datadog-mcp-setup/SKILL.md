---
name: datadog-mcp-setup
description: Select the correct Datadog MCP URL/domain for your region. Use as the next step after installing the plugin, or when configuring region/URL or guiding post-install setup.
---

# Datadog MCP setup — select your region

This skill helps you pick the right MCP endpoint for your Datadog region. Run it **after installing the plugin** so the agent can configure `DD_MCP_DOMAIN` and your MCP config for you.

## Region → domain table

Use **only** the `DD_MCP_DOMAIN` environment variable (domain only, not full URL). Map your region to the corresponding MCP domain:

| Region | MCP domain (`DD_MCP_DOMAIN`)   |
|--------|--------------------------------|
| us1    | mcp.datadoghq.com              |
| us3    | mcp.us3.datadoghq.com          |
| us5    | mcp.us5.datadoghq.com          |
| eu1    | mcp.eu1.datadoghq.com          |
| ap1    | mcp.ap1.datadoghq.com          |
| ap2    | mcp.ap2.datadoghq.com          |

## Flow the agent MUST follow

### Step 1 – Prompt

Ask the user which Datadog region they are in (e.g. US1, US3, EU1, AP1, AP2). Show the table above in your message so the user can pick by region code.

### Step 2 – Parse

Accept region in common forms (e.g. us1, US1, us3, EU1). Map to the corresponding MCP domain from the table (e.g. us1 → `mcp.datadoghq.com`, us3 → `mcp.us3.datadoghq.com`).

### Step 3 – Use DD_MCP_DOMAIN only

- **mcp.json:** Ensure the MCP config (e.g. workspace `.cursor/mcp.json`) uses the canonical form with **only** `DD_MCP_DOMAIN` — no other env vars, no hardcoded full URLs. Set the `datadog` server `url` to:

  ```
  https://${DD_MCP_DOMAIN:-mcp.datadoghq.com}/api/unstable/mcp-server/mcp?toolsets=core,visualizations
  ```

  If the file does not exist, create it with this structure. Preserve existing `mcpServers`; only add or update the `datadog` entry.

- **Set DD_MCP_DOMAIN to the user's value:** Set the chosen domain (e.g. `mcp.us3.datadoghq.com`) as `DD_MCP_DOMAIN` so the plugin uses it. Do this by either:
  - Creating or editing a `.env` (or env file the user's Cursor/plugin reads) in the project with: `DD_MCP_DOMAIN=<selected_domain>`, or
  - Giving the user exact instructions to run in their shell, e.g. `export DD_MCP_DOMAIN=mcp.us3.datadoghq.com` (and to add it to their profile if they want it persistent).

### Step 4 – Confirm

Tell the user that mcp.json is set to use `DD_MCP_DOMAIN`, that `DD_MCP_DOMAIN` has been set to their chosen domain (or how to set it), and that they may need to restart Cursor or reload the MCP server.

## Troubleshooting

- **Wrong region:** Run the skill again and pick a different region; set `DD_MCP_DOMAIN` to the new domain (or update .env / export).
- **Config file location:** Cursor typically uses `.cursor/mcp.json` in the workspace.
- Use **only** `DD_MCP_DOMAIN`; do not introduce or reference `DD_MCP_SERVER_URL` or full-URL env vars.

## References

- [Plugin mcp.json](https://github.com/datadog-labs/cursor-plugin/blob/main/mcp.json)
- README Quick Start and [Wrong Datadog domain on first connection](https://github.com/datadog-labs/cursor-plugin#wrong-datadog-domain-on-first-connection) troubleshooting
