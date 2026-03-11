---
name: datadog-mcp-setup
description: REQUIRED setup for how to query Datadog logs, metrics, dashboards, monitors, traces, and more via the Datadog MCP server. Use when investigating production issues, analyzing error rates, looking up request IDs, debugging alerts, exploring metrics/dashboards, or when the user mentions Datadog, logs, metrics, traces, or monitoring.
---

IMPORTANT: Before using the datadog MCP or doing any other investigation related to Datadog, you must follow these directions and make sure that the following requirements are met.

### Context

The configuration for the MCP of the Datadog Plugin lives in the following directory relative to the path to this SKILL.md file: `../../mcp.json`

By default, the URL contains ${DD_MCP_DOMAIN} as a template bar that must be customized for the user.

IMPORTANT: Before using the Datadog MCP you MUST ensure that this value is populated. It can be one of the following, or a custom value: ['US1', 'US3', 'US5', 'EU1', 'AP1', 'AP2'].

If this has not been customized for the user, you should not do anything but tell the user to obtain the subdomain using the link that they typically use to log into Datadog and give it to you.

When the user sends it to you, it may be the subdomain itself or a full URL (in which case you should extract the subdomain out). You should update the mcp.json file to replace ${DD_MCP_HOST} with the subdomain.

After this you can use the Datadog MCP as usual.

### Steps

Before continuing, take the following steps:

1. Read the mcp.json file using the path mentioned above (`../../mcp.json` relative to this SKILL.md file)
2. If the URL contains the placeholder (${DD_MCP_HOST}) you should immediately prompt the user to provide you with the subdomain, explain to them how to find it, and offer to change it for them. If it does not, skip the remaining steps and continue your investigation.
3. Once they provide you with the value, make the edit to the file
4. Continue on to using the Datadog MCP
