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
3. The PLUGIN itself is a thin shim (triggers, agents, commands)
   that rarely changes — usually nothing to do. When a plugin
   update IS announced, third-party marketplaces do not reliably
   auto-update, so per surface: terminal Claude Code:
   `claude plugin marketplace update tendrilapp` FIRST (the update
   command compares against a cached clone and can falsely report
   already-latest), then `claude plugin update tendril`, then
   `/reload-plugins`. VS Code extension: the `/plugins` panel has
   no update button — uninstall tendril, reinstall it from the
   marketplace, then close and reopen the chat panel. Desktop
   app: no plugin management; quit and relaunch Claude Desktop.
4. Confirm with the `tendril_doctor` MCP tool: report the version
   line. Never claim the update succeeded without that confirmation.
