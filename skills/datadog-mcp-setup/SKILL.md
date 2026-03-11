---
name: datadog-mcp-setup
description: REQUIRED setup for how to query Datadog logs, metrics, dashboards, monitors, traces, and more via the Datadog MCP server. IMPORTANT If you want to use Datadog, read these instructions BEFORE checking for what tools are available for Datadog as that MAY NOT WORK.
---

IMPORTANT: Before using the datadog MCP or doing any other investigation related to Datadog, you must follow these directions and make sure that the following requirements are met.

### Context

The configuration for the MCP of the Datadog Plugin lives in the following directory relative to the path to this SKILL.md file: `../../mcp.json`

In a non-configured state, the URL contains ${DD_MCP_DOMAIN} as a template bar that must be customized for the user.

IMPORTANT: Before using the Datadog MCP you MUST ensure that this value is populated. It must be one of the following:

- us1: mcp.datadoghq.com
- us3: mcp.us3.datadoghq.com
- us5: mcp.us5.datadoghq.com
- eu1: mcp.datadoghq.eu
- ap1: mcp.ap1.datadoghq.com
- ap2: mcp.ap2.datadoghq.com

If this has not been customized for the user, you should not do anything but tell the user to obtain the domain using the link that they typically use to log into Datadog and give it to you, or choose one form the list. DO NOT tell the user to edit the file themselves, you should ask for the information and change it for them.

When the user sends it to you, it may be the MCP domain/subdomain itself or a full URL (in which case you should extract the subdomain out and match it to one of the MCP domains above). You should update the mcp.json file to replace ${DD_MCP_DOMAIN} with the matched MCP domain.

The user may provide you with a URL that does not match any of the the above domains. In this case, ask them for the region that their organization uses. Let them know that if they don't know, they can ask their admin or reach out to support@datadoghq.com.

### Steps

Before continuing, take the following steps:

1. Read the mcp.json file in the plugin directory. This will in the grandparent directory relative to this SKILL.md file (`../../mcp.json`).
2. If the URL contains the placeholder (${DD_MCP_DOMAIN}) you should immediately prompt the user to provide you with the MCP domain (showing them the options above), explain to them how to find it, and offer to change it for them. If ${DD_MCP_DOMAIN} does not exist in the file, skip the remaining steps and continue your investigation.
   IMPORTANT: Do NOT continue to read files or check MCP tool schemas if DD_MCP_DOMAIN is included in the file. It is CRUCIAL that you provide this information to the user so they can give you the correct value, otherwise the MCP will not work.
3. Once they provide you with the value, make the edit to the file. Let the user know that they need to turn the MCP server off and on again by going to Cursor Settings (using the gear in the top-right or typing "Cursor Settings" in the Command Palette) > "Tools & MCP" > "Plugin MCP Servers".
4. Continue on to using the Datadog MCP
