---
name: datadog-mcp-setup
description: Select the correct Datadog MCP URL/domain for your region. Use as the next step after installing the plugin, or when configuring region/URL or guiding post-install setup.
---

# Datadog MCP setup — select your region

This skill helps you pick the right MCP endpoint for your Datadog region. Run it **after installing the Datadog Cursor plugin** so the agent can configure `DD_MCP_DOMAIN` and your MCP config for you.

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

## Environment variables

The plugin's **mcp.json** is already configured to read from environment variables. For example:

```json
{
  "mcpServers": {
    "datadog": {
      "url": "https://${DD_MCP_DOMAIN:-mcp.datadoghq.com}/api/unstable/mcp-server/mcp?toolsets=core,visualizations"
    }
  }
}
```

**Before using the MCP server, make sure the following environment variable is set in your shell:**

- **DD_MCP_DOMAIN** — The MCP hostname for your region (e.g. `mcp.datadoghq.com`, `mcp.us3.datadoghq.com`). Use the value for your region from the table above.

For example, add it to your shell profile (e.g. ~/.zshrc or ~/.bashrc):

```bash
export DD_MCP_DOMAIN="mcp.datadoghq.com"
```

(Use the value for your region from the table above.)

Then restart Cursor (or reload the window) so it picks up the new environment variable.

## Flow the agent MUST follow

**Help the customer set up their local shell profile.** Assist by editing the profile file (e.g. append the export line to ~/.zshrc or ~/.bashrc) or guide them step-by-step so the variable is added to their profile. Detect or ask which shell they use (zsh/bash), then append `export DD_MCP_DOMAIN="<selected_domain>"` to ~/.zshrc or ~/.bashrc so the variable is set when Cursor starts.

### Step 1 – Ensure Datadog MCP is not already set up elsewhere

Before prompting for region, check whether the Datadog MCP server is already configured in Cursor via other methods (e.g. a manual entry in `.cursor/mcp.json` or `~/.cursor/mcp.json`, another plugin, or Cursor MCP settings).

- **If Datadog MCP is already set up:** Do **not** proceed with the rest of this skill. Tell the user that the Datadog MCP server is already configured. Explain that running this skill would create another MCP server and could cause duplicate tools or conflicts. If they want to use the **Datadog Marketplace plugin** instead, ask them to disable or remove the other Datadog MCP server(s) first, then run this skill again or complete plugin setup.
- **If Datadog MCP is not set up (or only the Marketplace plugin is present):** Continue to Step 2.

### Step 2 – Prompt

Ask the user which Datadog region they are in (e.g. US1, US3, US5, EU1, AP1, AP2). Show the table above in your message so the user can pick by region code.

### Step 3 – Parse

Accept region in common forms (e.g. US1, US3, US5, EU1, AP1, AP2). Map to the corresponding MCP domain from the table (e.g. us1 → `mcp.datadoghq.com`, us3 → `mcp.us3.datadoghq.com`).

### Step 4 – Use DD_MCP_DOMAIN only

- **mcp.json:** Ensure the MCP config (e.g. workspace `.cursor/mcp.json`) uses the canonical form with **only** `DD_MCP_DOMAIN` — no other env vars, no hardcoded full URLs. Set the `datadog` server `url` to:

```json
{
  "mcpServers": {
    "datadog": {
      "url": "https://${DD_MCP_DOMAIN:-mcp.datadoghq.com}/api/unstable/mcp-server/mcp?toolsets=core,visualizations"
    }
  }
}
```

  If the file does not exist, create it with this structure. Preserve existing `mcpServers`; only add or update the `datadog` entry.

- **Set DD_MCP_DOMAIN in the customer's local shell profile:**  Help set up the user's **local shell profile**. Actively assist by editing the profile file (e.g. append the export line to ~/.zshrc or ~/.bashrc) or guide them step-by-step.
  - Detect or ask which shell they use (zsh vs bash), then target **~/.zshrc** or **~/.bashrc** (or both if unclear; e.g. add to ~/.zshrc, or ~/.bashrc if they use bash).
  - **Append** the line to the customer's profile: `export DD_MCP_DOMAIN="<selected_domain>"` where `<selected_domain>` is the value from the region table (e.g. `mcp.us3.datadoghq.com`). Prefer editing the file (read the profile, append the line, write it back) so the customer's local shell profile is actually updated.
  - If the user prefers not to edit the profile, offer a one-off `export` command for the current terminal and remind them it won't persist unless added to the profile.
  - **Alternative:** If the user explicitly wants a project-local env file instead of the shell profile, you may create or edit a `.env` in the project with `DD_MCP_DOMAIN=<selected_domain>`. Do not present this as the default.
  - After changing the profile, remind the user to **restart Cursor** (or reload the window) so Cursor picks up the new environment variable.

### Step 5 – Confirm

Confirm that (1) mcp.json is set to use `DD_MCP_DOMAIN`, (2) `DD_MCP_DOMAIN` has been added to their shell profile (e.g. ~/.zshrc or ~/.bashrc) with the chosen domain, and (3) they should **restart Cursor or reload the window** so the new variable is picked up.

## Troubleshooting

- **Wrong region:** Run the skill again and pick a different region; update the `export DD_MCP_DOMAIN=...` line in **~/.zshrc** or **~/.bashrc** with the new domain, then restart Cursor (or reload the window).
- **Config file location:** Cursor typically uses `.cursor/mcp.json` in the workspace.
- **Cursor reads env vars from the shell used to launch it;** adding to ~/.zshrc or ~/.bashrc ensures the variable is set when Cursor starts.
- Use **only** `DD_MCP_DOMAIN`; do not introduce or reference `DD_MCP_SERVER_URL` or full-URL env vars.

## References

- [Plugin mcp.json](https://github.com/datadog-labs/cursor-plugin/blob/main/mcp.json)
- README Quick Start and [Wrong Datadog domain on first connection](https://github.com/datadog-labs/cursor-plugin#wrong-datadog-domain-on-first-connection) troubleshooting
