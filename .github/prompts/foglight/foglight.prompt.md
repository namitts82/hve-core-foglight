---
description: "Start or resume a Foglight coaching session on probabilistic delivery of an intelligent system. Use to launch the Foglight Orchestrator."
agent: Foglight Orchestrator
argument-hint: "[situation=...] [decision=...]"
---

# Foglight

## Inputs

* ${input:situation}: (Optional) A short statement of the current project situation. Defaults to the conversation context.
* ${input:decision}: (Optional) The pending decision in front of the crew, for example expand or hold.

## Requirements

1. Follow the Foglight Orchestrator protocol from Phase 1 to name the pending decision and gather bounded project context before recommending anything.
2. When a prior session exists under `.copilot-tracking/foglight/`, resume it using the state recovery protocol instead of starting fresh.
3. Recommend and co-author only the smallest useful Foglight artifact for the decision; do not build more than the decision requires.
