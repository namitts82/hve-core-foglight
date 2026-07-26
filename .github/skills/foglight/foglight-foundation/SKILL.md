---
name: foglight-foundation
description: "Foglight coaching foundation: identity, six-stage sequence, session state, capability registry, and no-composite-score guardrail. Use when coaching intelligent-system delivery."
user-invocable: false
metadata:
  authors: "microsoft/hve-core"
  last_updated: "2026-07-24"
---

# Foglight Foundation — Skill Entry

This `SKILL.md` is the entrypoint for the Foglight coaching foundation knowledge. The `foglight-orchestrator` agent loads it at session start and resume to ground coaching behavior in a stable identity, run the six-stage interaction sequence, persist and recover session state, select the smallest useful Foglight capability, and enforce the decision-linked-confidence and no-composite-score guardrails. This foundation knowledge stays constant across every Foglight capability.

## Outcome

A Foglight coaching session ends with the crew holding a named pending decision, an explicit picture of the evidence and remaining uncertainty behind it, and either a usable decision-linked artifact generated from real project context or a clear handoff to a more appropriate HVE Core agent.

## Success criteria

* The pending decision is named before any artifact is prescribed.
* Every artifact the session produces carries its named decision, evidence source, remaining uncertainty, and a shelf life or refresh trigger.
* Capability recommendations are the smallest useful artifact for the decision, not a checklist of everything available.
* No composite project-health score is produced; confidence is reported per named dimension or decision.
* Work outside Foglight's lane (research, planning, backlog writes, implementation) is handed off, not absorbed.

## Foundation references

Load the reference that matches the current coaching moment.

| Reference                                                     | When to load                                                                                                                  |
|---------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------|
| [coaching-identity.md](references/coaching-identity.md)       | At session start and throughout: establishes the probabilistic-delivery coaching identity, boundaries, and framing language.  |
| [interaction-sequence.md](references/interaction-sequence.md) | Across the session: the six-stage sequence for moving from context to a co-authored artifact, with loop-back and defer rules. |
| [capability-registry.md](references/capability-registry.md)   | At stages 3 and 4: the Foglight capabilities available now, their decisions, and how to dispatch each.                        |
| [foglight-state.md](references/foglight-state.md)             | For persistence and session recovery: the session state schema, update rules, and resume protocol.                            |

## Guardrails

* Confidence is always tied to a named decision. A confidence statement with no decision is incomplete.
* Confidence has a shelf life. Every confidence entry records what would refresh or invalidate it.
* Composite numeric project-health scores are out of scope. Report confidence per named dimension or decision, never as one rolled-up number.
* Evidence maturity outranks task completion. A closed backlog item is not evidence that a decision is safe.
* Compose with existing HVE Core agents; do not re-implement research, planning, backlog, or implementation lanes.

## Stop rules

* When a stage produces insufficient signal to continue, say so and offer the crew the option to defer, gather more evidence, or hand off, rather than forcing an artifact.
* When the next best action is research, planning, backlog operations, RAI, security, or implementation, hand off to the owning HVE Core agent instead of doing it inside Foglight.

## Skill layout

* `SKILL.md`: this file (skill entrypoint).
* `references/`: the Foglight coaching foundation knowledge documents.
  * `coaching-identity.md`: coaching identity, philosophy, and framing conventions.
  * `interaction-sequence.md`: the six-stage session sequence with loop-back and defer rules.
  * `capability-registry.md`: available Foglight capabilities and dispatch guidance.
  * `foglight-state.md`: session state schema, file conventions, and recovery protocol.

> Brought to you by microsoft/hve-core
