# Tendril — Claude Code plugin marketplace

[Tendril](https://www.npmjs.com/package/@tendrilapp/cli) turns Figma
design systems into pixel-verified React components: it records the
design as ground truth, generates the component, and certifies the
result against the recording with a local ruler.

## Install (inside Claude Code)

```
/plugin marketplace add TendrilApp/claude-plugin
/plugin install tendril@tendrilapp
```

One install delivers the Tendril MCP server and the tendril skill
together. Requires Node ≥ 20 and Google Chrome; `tendril doctor`
(from the `@tendrilapp/cli` npm package) prints a readiness report.

Verification (`tendril verify`) is local, account-less, and
network-less — always.
