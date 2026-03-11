---
name: datadog-mcp-setup
description: REQUIRED if the Datadog MCP Server is not available. The Datadog MCP Server is used for investigating production issues, analyzing error rates, looking up request IDs, debugging alerts, exploring metrics/dashboards, or when the user mentions Datadog, logs, metrics, traces, or monitoring.
---

IMPORTANT: If the Datadog MCP Server is not available when doing any investigation related to Datadog, you must follow these directions and make sure that the MCP Server is available.

## Context

The configuration for the MCP Server of the Datadog Plugin lives in the following directory relative to the path to this SKILL.md file: `../../mcp.json`.

That file contains the URL for the Datadog MCP Server, which used the following variables and defaults:

- `${DD_MCP_DOMAIN}`: The domain for the MCP server.
  - When the plugin is installed, the domain is `mcp.datadoghq.com` as in `${DD_MCP_DOMAIN:-mcp.datadoghq.com}`.
  - The domain may have been customize since installation.

## Steps to take if the MCP Server is Unavailable

- **DO NOT ASK** the user to use the Datadog UI in the Browser: instead guide the user to fix the MCP Server.
- Read the domain (i.e, `${DD_MCP_DOMAIN-<domain>}`) set on the `../../mcp.json` file.
- Ask the user to confirm if the Datadog MCP Server is enabled and authenticated and if the domain read above is correct.

Depending on the answer of the user, use one of the flows in the following sections.

### View the MCP Server enablement and authentication state

- Ask the user to open the "Tools & MCP" section of the Cursor Settings. They can do that by clicking on the gear icon in the left sidebar and then on "Tools & MCP" in the dropdown menu or by running the "Cursor Settings: Tools & MCP" command in the Command Palette.
- Once they are in the "Tools & MCP" section, the user should verify if the MCP Server is enabled and if they are authenticated. If the MCP Server is not enabled, they should enable it. If they are not authenticated, they should authenticate.
- If the MCP Server is enabled, the user may need to reauthenticate if there are issues with the current authentication. To do that the user needs to run the command "Cursor: Clear All MCP Tokens" in the Command Palette.

### Change the domain for the MCP Server

To change the domain for the MCP Server, you MUST ask the user to provide which Datadog domain should be used. The valid domains are for each Datadog region are as follows:

- us1: mcp.datadoghq.com
- us3: mcp.us3.datadoghq.com
- us5: mcp.us5.datadoghq.com
- eu1: mcp.datadoghq.eu
- ap1: mcp.ap1.datadoghq.com
- ap2: mcp.ap2.datadoghq.com

When the user provides the answer:

- Pick one of the valid domains provided above.
  - IMPORTANT: **do not move forward** if the provided answer does not allow you to determine the correct domain. Go back to the user and ask them for clarification until you can determine the correct domain.
- Once you have the correct domain, update the `../../mcp.json` file as follows replace **only** the default value of the DD_MCP_DOMAIN variable with the appropriate domain:
  - Do not remove the variable!
  - Example: if the correct domain is `mcp.us3.datadoghq.com`, you should replace `${DD_MCP_DOMAIN:-mcp.datadoghq.com}` by `${DD_MCP_DOMAIN:-mcp.us3.datadoghq.com}`. Notice that the current value may be not be `${DD_MCP_DOMAIN:-mcp.datadoghq.com}` if it was already customized, so make sure to only replace the default value and not the variable itself.

After changing the domain, you must ask the user to:
- Clear all MCP tokens by running the command "Cursor: Clear All MCP Tokens" in the Command Palette.
- Restart Cursor by either closing and reopening it or by running the "Developer: Reload Window" command in the Command Palette.

## Important Notes (you MUST read this)

- The flow described in the "Steps to take if the MCP Server is Unavailable" section also works if the user requests to change the domain for the MCP Server even if the MCP Server is currently available.
