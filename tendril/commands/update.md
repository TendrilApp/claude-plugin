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
3. The PLUGIN itself is a thin shim (triggers, agent pointers,
   commands) that changes only when its wiring changes, and it
   updates itself once auto-update is on for its marketplace.
   Only if doctor's plugin-skew line is not green: terminal Claude
   Code — `claude plugin marketplace update tendrilapp`, then
   `claude plugin update tendril`, then `/reload-plugins`; VS Code
   extension — `/plugins` → Marketplaces tab: refresh tendrilapp,
   then Plugins tab: uninstall tendril, reinstall it, close and
   reopen the chat panel. Then have the user turn auto-update on
   so it never recurs: `/plugin` → Marketplaces → tendrilapp →
   Enable auto-update.
4. Confirm with the `tendril_doctor` MCP tool: report the version
   line. Never claim the update succeeded without that confirmation.
