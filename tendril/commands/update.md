---
description: Update Tendril safely (CLI, npx cache, plugin) and confirm the version
---

Walk the user's machine to the latest Tendril, in this order, telling
the user each step:
1. `npm install -g @tendrilapp/cli@latest` (updates the global CLI;
   the alias `tendrilapp` follows automatically).
2. If the MCP server announced a stale version this session:
   `npm cache clean --force`, then tell the user to restart the
   session so the plugin's npx command re-resolves @latest.
3. Update the PLUGIN itself (skill/agents/commands do not
   auto-update on third-party marketplaces) — the command depends
   on the surface: terminal Claude Code: `claude plugin update
   tendril` (or the `/plugin` manager), then `/reload-plugins`.
   VS Code extension: type `/plugins` (plural) in the chat, find
   tendril under Installed, update it, then close and reopen the
   chat panel (the extension has no /reload-plugins). Desktop
   app: quit and relaunch Claude Desktop.
4. Confirm with the `tendril_doctor` MCP tool: report the version
   line. Never claim the update succeeded without that confirmation.
