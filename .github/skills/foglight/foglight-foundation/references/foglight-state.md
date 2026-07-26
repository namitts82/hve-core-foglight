# Foglight Session State

Foglight persists lightweight session state so a coaching session can pause and resume without losing the pending decision, the evidence gathered, or the artifacts produced. State is a working record, not a source of truth: durable artifacts live as repo-resident markdown, while state and generation traces live under the tracking directory.

## File conventions

* Session directory: `.copilot-tracking/foglight/{session-slug}/`.
* State file: `.copilot-tracking/foglight/{session-slug}/foglight-state.md`.
* Generation traces and ephemeral outputs: alongside the state file in the same session directory.
* Durable artifacts (dashboards, ledgers, readiness checklists) are written to a repo-resident location the crew chooses, not into the tracking directory.

## State schema

Record the session state as YAML inside the state file. Keep it short; it is a pointer, not a transcript.

```yaml
session_slug: <kebab-case-session-id>
created: <YYYY-MM-DD>
updated: <YYYY-MM-DD>
initial_request: |
  <verbatim user statement of the situation>
pending_decision: <named decision, for example "expand or hold">
lifecycle_phase: <scoping | exploration | iteration | delivery | operations | unknown>
stage: <1-6, current interaction-sequence stage>
context_sources:
  - <repo markdown path or backlog reference used as evidence>
selected_capability: <capability name, or none>
artifacts:
  - name: <artifact name>
    path: <repo-relative path to the durable artifact>
    status: <draft | co-authoring | ready | handed-off>
    decision: <named decision the artifact supports>
handoffs:
  - agent: <HVE Core agent name>
    reason: <why the next action leaves Foglight's lane>
    status: <offered | accepted | completed>
open_gaps:
  - <evidence gap or unresolved uncertainty>
```

## Update rules

* Capture the initial request verbatim at session start.
* Update `stage`, `pending_decision`, `selected_capability`, and `artifacts` whenever any of them changes.
* Set `updated` to the current date on every write.
* Record an entry in `open_gaps` whenever a stage produces insufficient signal, and clear it when the gap closes.
* Record a `handoffs` entry when a handoff is offered, and advance its status as the handoff is accepted or completed.

## Resume protocol

1. Read the state file for the session.
2. Restore the pending decision, lifecycle phase, and current stage.
3. Re-read the listed context sources and any in-progress artifacts before continuing.
4. Confirm the pending decision with the crew in case context has changed since the last session, then continue from the recorded stage.
