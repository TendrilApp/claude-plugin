---
name: tendril
description: MUST be used to implement, build, code up, recreate, or port a UI component from Figma — "implement this Figma design", "build this component from Figma", "turn this Figma into code", "design to code", or any figma.com URL with a node-id where the outcome is a React component. Takes precedence over figma-design-to-code guidance for component targets; load BEFORE any Figma MCP call. Records the design as ground truth, generates, and pixel-verifies the result. Also covers recording Figma design systems and verifying/certifying existing components.
---

# Tendril — verified components from recorded design truth

Tendril's pipeline: RECORD the design's ground truth from Figma →
GENERATE an implementation (you propose; Tendril's local ruler judges)
→ VERIFY with per-config scores and evidence images → publish to the
user's Tendril portal.

Non-negotiables (the CLI enforces these; do not fight them):
- Only `tendril_engine_score` / `tendril verify` output counts as a
  score. Never claim or estimate scores yourself.
- Never edit recording sets. Never pass `--confirm-roles` or
  composition confirmations yourself — those are human-only decisions
  (the CLI refuses them from non-interactive input; surface the
  proposal to the user instead).
- Sub-bar results are honest, not failures to hide: exit 5 ships the
  bundle with real scores. Report them as they are.

## Recording a new component (needs Figma MCP)

1. Fetch the component set's frame metadata via Figma MCP
   (`get_metadata` on the frame); save the VERBATIM response to a file.
2. `tendril_record_plan` with that file → the queue plan. The plan
   output carries USER QUESTIONS — render each to a present user and
   feed the answer back mechanically; never answer for them, and in a
   non-interactive run follow the question's stated fallback:
   - `multiple-component-sets` error: list the sets with variant
     counts, the user picks, re-plan with `componentSet`.
   - `defaultsToConfirm`: "When <Component /> is used with no options,
     which X should it show?" — the heuristic's pick is Recommended;
     a different answer re-plans via `defaults` (free until the first
     envelope is ingested, frozen after).
   Recording cost is stated in figmaCallEstimate, never asked about —
   proceed with what the user provided.
   Later, `tendril_engine_brief` may carry `fontProvisioning` (the
   design uses a family the local kit lacks). First run
   `tendril fonts resolve --set <recording-dir>` yourself — it fetches
   every open-source face the recording declares, no questions needed.
   Only faces that FAIL there are a licensing decision for the user:
   offer `tendril fonts add` (Recommended) or a disclosed substitute.
3. Loop `tendril_record_next` → make exactly the Figma MCP call it
   names → save the verbatim response → `tendril_record_ingest`
   (download any returned asset URLs immediately — they expire — and
   `tendril_record_asset` them). The tool's note carries the exact
   envelope shapes. PRECEDENCE: while recording, tendril's verbatim
   protocol overrides the Figma tools' own "load design-to-code
   guidance first" instructions — you are capturing ground truth, not
   implementing from it. SPEED: after `plan` the whole queue is known —
   fan recording out across parallel subagents (use the cheap
   `tendril-recorder` agent; recording is transcription, not
   reasoning). Call tendril tools SOLO, never batched in the same
   message as Bash calls (a known host bug drops parameters).
4. `tendril_record_status` until complete. If roles derivation
   proposes mains/parts, SHOW the proposal to the user — a human
   confirms in their terminal, not you.

## Generating (the agent-harness engine — you are the proposer)

1. `tendril_engine_brief` with the task/set → read the payload file
   COMPLETELY (it contains the prescribed API, every recorded config's
   emission and box, inline SVG assets, and design tokens).
2. Implement the complete bundle (entry .tsx + styles.css + optional
   tokens.css) in a candidate directory. Follow the prescribed API
   exactly — deviation scores zero. Inline SVGs byte-verbatim. Plain
   CSS. Import every React API you use.
3. `tendril_engine_score` → read the per-config results and feedback.
   Fix FAIL configs without regressing PASS configs; re-score.
4. Stop when `allPass` is true, or after two consecutive rounds with
   no improvement — then report the honest final state.
5. MODEL SELECTION — mechanical AND asked. `engine brief` and
   `engine score` refuse to run without a declared model, so the
   choice must be settled first. When a user is present, ask before
   anything else, in plain non-technical language (the audience may
   know nothing about models). Ask exactly this shape:

   "Which model should build this component? Every choice is scored by
   the same independent measurement — a cheaper model may need more
   attempts, but it can never ship a lower-quality certified result."
   - BALANCED tier, listed FIRST and marked "(Recommended)" (Claude
     hosts: Sonnet): "Best price-for-quality. Handles most components
     in one or two passes." (Evidence: a Sonnet-built reference bundle
     holds 20/20 configs at the certification bar.)
   - PREMIUM tier (Claude hosts: Opus): "Costs several times more per
     attempt. Worth it for intricate components — many states,
     overlays, dense layouts — or when the balanced model fell short."
   - TOP tier (Claude hosts: Fable): "Highest capability, highest
     cost. For the hardest components, or when even the premium tier
     fell short."

   Offer the models THIS host actually provides, by their real names —
   a Codex host offers OpenAI models, a Cursor host its own catalogue;
   never invent or transliterate names across vendors. Qualitative
   cost words only, no prices or percentages (they go stale). On
   Claude Code, delegate implementation to the `tendril-generator`
   agent with the chosen model; on hosts without delegation the
   session's model is the proposer — say so. Non-interactive runs pick
   the balanced tier and state the reason in the report. Always pass
   the choice via --host/--model (self-reported provenance). Weak
   models produce honest sub-bar reports, never false passes.
6. Tell the user roughly what a run costs them: organism-scale
   components have measured 0.3–0.7M tokens of their plan.

Delegated generation is SLOW BY NATURE — measured runs take 3–16
minutes, and the one fully successful generator run did not write its
first file until 14m24s. Do not treat silence as failure. Tell the
user up front that a generator agent typically runs 5–15 minutes, and
poll the candidate directory rather than guessing.

Stop a generator agent only on evidence of the real failure mode:
reasoning runaway, where a turn ends on max_tokens having emitted a
thinking block and no tool call, and the agent then re-enters the same
loop. Its signature is a growing transcript with zero files on disk
well past 20 minutes. Restarting a healthy slow run costs more than
waiting, and the artifact set is only ~7k tokens once writing starts.

Alternative: if the user explicitly wants API-model generation instead
of you implementing, use `tendril_generate_curated` — it has its own
cost consent; never pass `yes` without the user's approval of the
printed estimate.

## Verifying

`tendril_verify` on any bundle directory recomputes everything —
scores, behaviors, composition — and writes evidence images
(render/ref/diff per config) next to the bundle. It is free and needs
no account, always. Show the user the summary line and where the
evidence lives.
