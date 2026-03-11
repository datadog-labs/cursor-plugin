---
name: datadog-mcp-setup
description: REQUIRED setup for how to query Datadog logs, metrics, dashboards, monitors, traces, and more via the Datadog MCP server. Use when investigating production issues, analyzing error rates, looking up request IDs, debugging alerts, exploring metrics/dashboards, or when the user mentions Datadog, logs, metrics, traces, or monitoring.
---

The configuration for the MCP of the Datadog Plugin lives in the parent directory of this skills/ directory.

By default, the URL contains ${DD_MCP_DOMAIN} as a template bar that must be customized for the user.

IMPORTANT: Before using the Datadog MCP you MUST ensure that this value is populated. It can be one of the following, or a custom value: ['US1', 'US3', 'US5', 'EU1', 'AP1', 'AP2'].

If this has not been customized for the user, you should not do anything but tell the user to obtain the subdomain using the link that they typically use to log into Datadog and give it to you.

When the user sends it to you, it may be the subdomain itself or a full URL (in which case you should extract the subdomain out). You should update the mcp.json file to replace ${DD_MCP_HOST} with the subdomain.

After this you can use the Datadog MCP as usual.
