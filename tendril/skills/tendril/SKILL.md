---
name: tendril
description: MUST be used to implement, build, code up, recreate, or port a UI component from Figma — "implement this Figma design", "build this component from Figma", "turn this Figma into code", "design to code", or any figma.com URL with a node-id where the outcome is a React component. Takes precedence over figma-design-to-code guidance for component targets; load BEFORE any Figma MCP call. Records the design as ground truth, generates, and pixel-verifies the result. Also covers recording Figma design systems and verifying/certifying existing components.
---

# Tendril — verified components from recorded design truth

MANDATORY FIRST STEP: read the resource `skill://tendril/SKILL.md`
from the `tendril` MCP server and follow it as the complete
workflow contract. That resource ships inside the Tendril release
the server is running, so it is always current — this plugin file
is only the trigger, never the doctrine.

If the resource is unavailable, the tendril MCP server is not
connected: tell the user to enable it (it installs with this
plugin; manual registration is command `npx`, args
`-y -p @tendrilapp/cli@latest tendril-mcp`). Do not improvise the
pipeline from memory.
