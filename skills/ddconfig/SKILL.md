---
name: ddconfig
description: Configure the Datadog Cursor Plugin. Use to set the domain, switch organizations, or manage toolsets.
allowed-tools: Bash(./scripts/*.mjs)
---

# Datadog Plugin Configuration

## CRITICAL: Output Behavior

**YOU MUST display all script output in your message text. ALL script output is important and MUST be displayed to the user.** After running any script with Bash:

1. Copy the entire output from `<function_results>` into your message text
2. Do NOT add commentary like "I'll run..." or "Here's the menu:"
3. Do NOT reformat - scripts already format the UI
4. The user CANNOT see tool results unless you output them in your message
5. Script output typically includes a list of options followed by a question. You must include ALL script output in your response, including the full list of options.

Why this matters: Tool results are collapsed by default in the UI. If you don't copy the output into your response text, the user sees nothing and doesn't know what to do next.

## Scripts

**IMPORTANT:** ALL scripts are located in this skill's `./scripts` subdirectory. YOU CANNOT FORGET THIS.

Scripts output formatted UI by default. Use `--json` flag ONLY when you need structured data to call action scripts.

- **Entry:** `start-ddconfig.mjs`
- **Query:** `get-status.mjs`, `get-domains.mjs`, `get-organizations.mjs`, `get-toolsets.mjs`
- **Action:** `set-domain.mjs <domain>`, `switch-organization.mjs "<connId>" "<orgId>" "<name>"`, `enable-toolsets.mjs <name1> <name2>...`
- **Reset:** `reset.mjs`

Any time `reset.mjs` is invoked, a warning **MUST** first be displayed and the command must be confirmed.

## Duplicate MCP Server Check

Before showing the menu, SILENTLY (without telling the user), check for duplicate Datadog MCP servers. This is a PURE REASONING STEP — do NOT call any tool for it. Instead, inspect the list of tools already available in your context (e.g. the deferred tools list in your system prompt).

Steps:

1. Collect all tool names prefixed with `mcp__plugin_datadog_mcp__` (these belong to `plugin:datadog:mcp`). If there are none (all toolsets disabled), skip this check entirely.
2. Extract just the suffix of each name (e.g. `analyze_datadog_logs` from `mcp__plugin_datadog_mcp__analyze_datadog_logs`).
3. Check every other MCP server's tools in your context — any server whose tools share the same suffix names is a conflict.

If overlapping tools are found, before proceeding with the menu:

- Warn the user that they have Datadog tools from other MCP servers that overlap with this plugin, listing the conflicting server names (group tools by server using the tool naming convention - e.g. `mcp__<server>__<tool>`). Indicate that having duplicate tools active simultaneously can cause conflicts (authentication issues, tools called on the wrong server), and recommend them to disable or remove the non-plugin server(s) and then restart Cursor.
- Then ask the user if they want to continue with the plugin configuration or fix the issue first.
- **STOP HERE** Do NOT proceed to the Main Menu Flow until the user responds.

## Main Menu Flow

1. **[REQUIRED FIRST]** Perform the "Duplicate MCP Server Check" section above. Do NOT continue to the next step until this check is done and any conflict is resolved or the user chooses to proceed.
2. Run `start-ddconfig.mjs`
3. **Display the output exactly as-is** — the script formats the UI and includes all necessary guidance
4. Wait for the user's response
5. **Parse their choice from the numbered options shown in the output.** The script may show 2, 3, 4, or more options depending on connection state. Parse flexibly:
   - Numbers: "1", "2", etc.
   - Full names: "domain", "organization", etc.
   - Partial matches: "org", "orgs", etc.
   - Shortcuts: "d", "o", etc.
6. Route to appropriate flow below based on their choice

## Domain Set Flow

1. Run `get-domains.mjs`
2. **Display the output as-is**
3. Wait for the user's response
4. **Parse their choice flexibly** (numbers map to list, names match domains)
5. Execute: `set-domain.mjs <domain>`

## Organization Switch Flow

1. Run `get-organizations.mjs`
2. **Display the output as-is**
3. If no switchable orgs, inform user and stop
4. Wait for the user's response
5. **Parse their choice flexibly**:
   - Numbers: map to organization list
   - Names: case-insensitive partial match on org name
   - Example: "demo" matches "Datadog Demo (11287)"
6. To get structured data for the action script, run `get-organizations.mjs --json`
7. Extract connectionId, id, name for the selected org
8. Execute: `switch-organization.mjs "<connectionId>" "<orgId>" "<name>"` (all 3 required, quoted)

## Toolsets Flow

1. Run `get-toolsets.mjs`
2. **Display the output as-is**
3. Wait for the user's response
4. **Parse their selection flexibly** (preserve this exact parsing logic):
   - **Incremental mode** (if contains + or -):
     - `+logs +metrics` → add these toolsets
     - `-apm` → remove this toolset
     - `+logs -apm` → mix of both
   - **Absolute mode** (no +/-):
     - `logs metrics` → replace with exactly these
5. Validate names against available toolsets
6. Execute: `enable-toolsets.mjs <name1> <name2>...` (space-separated args)

## Reset Flow

This flow **MUST** be followed exactly.

1. Warn the user that _ALL_ settings and connections for the Datadog plugin will be cleared and they will need to sign in again and reconfigure from scratch. Ask them to confirm.
2. Wait for the user's confirmation response
3. If not confirmed, acknowledge and stop — do not proceed
4. If confirmed, execute: `reset.mjs`
5. **Display the output as-is**
6. Check whether the output contains reconnect instructions (e.g. a command to restart the MCP server):
   - **If it does:** tell the user to follow those instructions now
   - **If it does not:** the plugin reconnects automatically — do NOT mention reconnecting at all

## Connect Flow

1. Run `get-status.mjs connect`
2. **Display the output as-is**

## Quick Info Queries

If user asks about current connection without requesting configuration:

- Run `get-status.mjs` (outputs connection status, not full menu)
- Parse the output to answer their question

## Other Notes

- **After config changes:** Backend auto-reconnects, may trigger OAuth if switching to a domain or organization without a cached connection.
- **If script hangs or fails:** Show the error and suggest running the `/ddconfig` command. The last resource is to restart the Datadog MCP server from Cursor's MCP settings or restart Cursor.

