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
reopen the chat panel. **While you are there, enable auto-update
for this marketplace** — third-party marketplaces default to off;
enabling it once means future plugin updates arrive without any
manual step.

The plugin is a convenience layer: the MCP server, its tools, its
slash-command prompts, and the always-current workflow contract
(served as the resource `skill://tendril/SKILL.md`) all update
automatically with every session via npm — no plugin required.

One install delivers the Tendril MCP server and the tendril skill
together. Requires Node ≥ 20 and Google Chrome; `tendril doctor`
(from the `@tendrilapp/cli` npm package) prints a readiness report.

**Staying up to date:** the MCP server always runs the latest
published Tendril release automatically. The plugin's own content
(skill, agents, commands) does not auto-update on third-party
marketplaces — update it per surface:

- **Terminal Claude Code:** `claude plugin update tendril` (or the
  `/plugin` manager), then `/reload-plugins`.
- **VS Code extension:** type `/plugins` in the chat → Installed →
  update tendril → close and reopen the chat panel. (Install works
  the same way: `/plugins` → Marketplaces → add
  `TendrilApp/claude-plugin` → Install.)
- **Claude Desktop:** plugin management is not available there yet;
  quit and relaunch to pick up cached updates.

Verification (`tendril verify`) is local, account-less, and
network-less — always.
