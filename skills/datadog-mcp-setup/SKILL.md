---
name: datadog-mcp-setup
description: REQUIRED setup for how to query Datadog logs, metrics, dashboards, monitors, traces, and more via the Datadog MCP server. Use when investigating production issues, analyzing error rates, looking up request IDs, debugging alerts, exploring metrics/dashboards, or when the user mentions Datadog, logs, metrics, traces, or monitoring.
---

IMPORTANT: Before using the datadog MCP or doing any other investigation related to Datadog, you must follow these directions and make sure that the following requirements are met.

### Context

The configuration for the MCP of the Datadog Plugin lives in the following directory relative to the path to this SKILL.md file: `../../mcp.json`

By default, the URL contains ${DD_MCP_DOMAIN} as a template placeholder that must be customized for the user.

IMPORTANT: Before using the Datadog MCP you MUST ensure that this value is populated. It must be the full MCP hostname for the user's Datadog site. The valid values per site are:

| Site | DD_MCP_DOMAIN |
|------|---------------|
| US1  | mcp.datadoghq.com |
| US3  | mcp.us3.datadoghq.com |
| US5  | mcp.us5.datadoghq.com |
| EU1  | mcp.datadoghq.eu |
| AP1  | mcp.ap1.datadoghq.com |
| AP2  | mcp.ap2.datadoghq.com |

If this has not been customized for the user, you should not do anything but tell the user to obtain the domain using the link that they typically use to log into Datadog and give it to you.

When the user sends it to you, it may be the full MCP domain or a Datadog app URL (in which case you should derive the correct MCP domain from the table above). You should update the mcp.json file to replace ${DD_MCP_DOMAIN} with the correct domain.

After this you can use the Datadog MCP as usual.

### Steps

Before continuing, take the following steps:

1. Read the mcp.json file using the path mentioned above (`../../mcp.json` relative to this SKILL.md file)
2. If the URL contains the placeholder (${DD_MCP_DOMAIN}) you should immediately prompt the user to provide you with their Datadog site URL, explain to them how to find it, and offer to change it for them. If it does not, skip the remaining steps and continue your investigation. IMPORTANT: Do NOT continue to read files or check MCPs. It is CRUCIAL that you provide this information to the user so they can give you the correct value. You will not find it on your own.
3. Once they provide you with the value, make the edit to the file. Let the user know that they need to turn the MCP server off and on again by going to Cursor Settings -> MCP.
4. Continue on to using the Datadog MCP
