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
3. `/plugin update tendril` for skill/agent/command updates (plugin
   content does not auto-update on third-party marketplaces).
4. Confirm with the `tendril_doctor` MCP tool: report the version
   line. Never claim the update succeeded without that confirmation.
