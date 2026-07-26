# Foglight Capability Registry

This registry lists the Foglight capabilities available now, the decision each one serves, and how to dispatch it. Use it at Stage 3 (offer areas of expertise) to surface a short menu of relevant lanes, and at Stage 4 (prescribe relevant tools) to recommend the single smallest useful artifact for the chosen decision.

The registry grows as the pilot pack expands. Only capabilities listed here are dispatchable today; the rest of the Foglight catalogue is future work and must not be promised as available.

## Available capabilities

| Capability          | Lane                              | Serves the decision                                                  | Dispatch                                                     |
|---------------------|-----------------------------------|---------------------------------------------------------------------|-------------------------------------------------------------|
| Confidence Dashboard | Evidence maturity and readiness   | Any go or hold decision where several independent uncertainties are being collapsed into one status signal (expand or hold, roll out to a new tenant, swap a model, change a prompt). | Dispatch the `confidence-dashboard` subagent with the named decision, the project context sources, and, on an update, the path to the existing dashboard artifact. |

## Selection guidance

* Match the capability to the pending decision, not to a phase or a habit. Recommend the Confidence Dashboard when the crew is treating a single green light as if it meant safe to proceed.
* Recommend one artifact at a time. If a second artifact seems warranted, name it as a candidate follow-on rather than bundling both.
* When no listed capability fits the decision, say so plainly and offer to hand off to the owning HVE Core agent rather than stretching a capability past its purpose.

## Dispatch contract

* Pass the named decision, the project context sources (repo markdown paths and any read-only backlog context), and the run mode (generate or update).
* On an update, pass the path to the existing artifact so the subagent revises it in place and reports what moved and why.
* Receive back the artifact path, a per-dimension or per-decision status summary, and any evidence gaps that suggest a handoff.
