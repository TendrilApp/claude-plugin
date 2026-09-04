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

Then, once, turn on auto-update for this marketplace so the plugin
keeps itself current: `/plugin` → Marketplaces → `tendrilapp` →
**Enable auto-update**. (Claude Code turns it off by default for
marketplaces it does not run itself.)

**Everything that matters updates itself.** The MCP server, its
tools, its prompts, the always-current workflow contract (served
as the resource `skill://tendril/SKILL.md`) and the helper agents'
instructions (served by the `tendril_doctrine` tool) ride the npm
`@latest` channel and refresh at every session start. The plugin
itself is a thin, stable shim — triggers, agent registrations,
slash commands — that changes only when its wiring changes, and
with auto-update on it follows those changes by itself.

Requires Node ≥ 20 and Google Chrome; `tendril doctor` (from the
`@tendrilapp/cli` npm package) prints a readiness report, including
whether this plugin is still compatible with the running release.

**If doctor says the plugin is stale** (it will not on a release
that only changed the server), update it once by hand:

- **Terminal Claude Code:** `claude plugin marketplace update
  tendrilapp`, then `claude plugin update tendril`, then
  `/reload-plugins`.
- **VS Code extension:** `/plugins` → Marketplaces tab: refresh
  `tendrilapp` first, then Plugins tab: uninstall tendril, reinstall
  it, close and reopen the chat panel.
- **Claude Desktop:** the plugin browser under the **+** button
  manages plugins; otherwise quit and relaunch.

Verification (`tendril verify`) is local, account-less, and
network-less — always.
