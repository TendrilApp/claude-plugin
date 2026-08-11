# Tendril — Claude Code plugin marketplace

[Tendril](https://www.npmjs.com/package/@tendrilapp/cli) turns Figma
design systems into pixel-verified React components: it records the
design as ground truth, generates the component, and certifies the
result against the recording with a local ruler.

## Install (inside Claude Code)

Terminal:

```
/plugin marketplace add TendrilApp/claude-plugin
/plugin install tendril@tendrilapp
```

VS Code extension: type `/plugins` (or `/plugin`) in the chat →
Marketplaces → add `TendrilApp/claude-plugin` → Install tendril →
reopen the chat panel.

**Everything that matters updates itself.** The MCP server, its
tools, its prompts, and the always-current workflow contract
(served as the resource `skill://tendril/SKILL.md`) ride the npm
`@latest` channel and refresh at every session start. The plugin
itself is a thin, stable shim — triggers, helper agents, slash
commands — that rarely changes, so you almost never need to touch
it after install.

Requires Node ≥ 20 and Google Chrome; `tendril doctor` (from the
`@tendrilapp/cli` npm package) prints a readiness report.

**On the rare occasion a plugin update is announced** (third-party
marketplaces do not reliably auto-update in current Claude Code
builds):

- **Terminal Claude Code:** `claude plugin marketplace update
  tendrilapp` first (refreshes the catalog — without it the update
  command may falsely report already-latest), then
  `claude plugin update tendril`, then `/reload-plugins`.
- **VS Code extension:** the `/plugins` panel has no update button —
  uninstall tendril, reinstall it from the marketplace, then close
  and reopen the chat panel.
- **Claude Desktop:** no plugin management yet; quit and relaunch.

Verification (`tendril verify`) is local, account-less, and
network-less — always.
