---
description: Tendril readiness and version status (doctor + update check)
---

Run the `tendril_doctor` MCP tool (fall back to `tendril doctor` in the
shell if the tool is unavailable) and report to the user, concisely:
the installed version and whether an update is available (with the
one-line update command when it is), browser identity, font-cache
state, and Figma desktop MCP reachability — with each failing item's
remediation exactly as doctor printed it. Do not editorialize numbers;
doctor's output is the source of truth.
