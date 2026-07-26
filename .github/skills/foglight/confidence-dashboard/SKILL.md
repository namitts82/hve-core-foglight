---
name: confidence-dashboard
description: "Confidence Dashboard dimensions, levels, usage rules, and templates. Use when a single status signal hides real uncertainty behind a named go/hold decision."
metadata:
  authors: "microsoft/hve-core"
  last_updated: "2026-07-24"
---

# Confidence Dashboard — Skill Entry

This skill packages the knowledge and templates for the Confidence Dashboard, Foglight's first capability. The `confidence-dashboard` subagent loads it to generate or update a decision-linked dashboard from real project context. The dashboard decomposes program uncertainty into six named dimensions, assigns each a level based on evidence, and tracks movement over time so confidence movement, not activity, becomes the progress signal.

## Outcome

A crew facing a go or hold decision holds a dashboard that separates the strong dimensions from the genuinely uncertain ones, ties each level to the evidence behind it, and turns each weak dimension into a named next experiment, hypothesis, or check.

## Success criteria

* The dashboard is tied to one named decision, not to overall project health.
* All six dimensions are present, each with a level, its evidence, remaining uncertainty, and a next step.
* No composite or rolled-up score appears; the six dimensions stay separate.
* On an update, every changed dimension records the direction of movement, why it changed, and what the change means for the decision.
* The confidence legend appears in any stakeholder-facing rendering.

## The six dimensions

Each dimension captures a distinct category of risk that the crew tracks independently. Treating them as separate signals is the point; collapsing them into one number is the failure mode the dashboard exists to prevent.

| Dimension                | What it measures                                                                                            | Example evidence                                                                                |
|--------------------------|-------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------|
| Problem Confidence       | Whether the problem is clearly defined, scoped, and aligned with business objectives.                       | Agreed success criteria, documented problem statement, identified user need.                    |
| Data Confidence          | Whether available data is sufficient in volume, quality, coverage, and freshness for the intended use.      | Data profiling results, label accuracy audits, coverage analysis across required segments.      |
| Feasibility Confidence   | Whether a model-based approach can solve the problem within time, cost, and talent constraints.             | Baseline experiment results, proof-of-concept metrics, comparison against heuristic benchmarks. |
| Signal Confidence        | Whether model outputs are stable across runs, reproducible, and interpretable by downstream consumers.      | Metric variance across evaluation runs, inter-annotator agreement, slice-level consistency.     |
| Applicability Confidence | Whether the solution generalizes to the real operating environment, including edge cases and drift.         | Performance on held-out production data, domain-expert review of failures, robustness testing.  |
| Operational Confidence   | Whether the solution can be deployed, monitored, maintained, retrained, and governed in existing processes. | Deployment pipeline readiness, monitoring coverage, incident response plan, compliance review.  |

## The three levels

Levels are intentionally coarse to prevent false precision. A three-level scale forces a clear judgment instead of a 7-out-of-10 that hides ambiguity.

| Level          | Criteria                                                                                                              | Crew action                                                                                             |
|----------------|-----------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------|
| Low (red)      | Significant unknowns remain. Evidence is insufficient to support a confident decision on this dimension.              | Prioritize this dimension. Direct effort toward the missing evidence. Flag it in reports.               |
| Medium (amber) | Partial evidence exists. Key risks are identified but not fully resolved. The direction is promising but unconfirmed. | Monitor actively. Track whether evidence is accumulating or stalling. Plan targeted work to close gaps. |
| High (green)   | Strong evidence base. Residual risks are understood and manageable within defined constraints.                        | Maintain monitoring. Shift focus to dimensions still at Low or Medium.                                  |

Confidence legend for stakeholder-facing renderings: red is Low, amber is Medium, green is High.

## Usage rules

* Initialize the dashboard at kickoff and update it at every learning-cycle exit.
* Confidence movement is the primary progress signal; activity without confidence movement is not progress.
* Every update states why confidence changed and what decision the change enables or blocks.
* A dimension that shows no movement beyond its declared refresh trigger or expected learning cadence is stalled; treat the stall as a signal and review the approach for that dimension.
* Include the confidence legend in every stakeholder-facing artifact that references the dashboard.
* The dashboard summarizes detailed technical reports into a decision-oriented view; it does not replace them.
* Never produce a composite score across the six dimensions. A gate recommendation reasons over the dimensions individually.

## Templates

Copy the template that matches the run, then populate it from project evidence. Full templates live in the referenced files so this entry stays compact.

| Template                                                           | Use for                                                                                          |
|--------------------------------------------------------------------|--------------------------------------------------------------------------------------------------|
| [primary-dashboard.md](templates/primary-dashboard.md)             | The core tracking artifact: one row per dimension, updated at every learning cycle.              |
| [status-change-view.md](templates/status-change-view.md)           | A stakeholder-facing update focused on movement and its implications.                            |
| [phase-gate-summary.md](templates/phase-gate-summary.md)           | A gate decision input consolidating each dimension's level, trend, evidence, and recommendation. |
| [confidence-movement-log.md](templates/confidence-movement-log.md) | A historical record of confidence changes for audit and retrospective purposes.                  |

## Gate logic

At a phase gate, reason over the dimensions individually rather than averaging them.

* A proceed recommendation requires all six dimensions at Medium or above with documented evidence.
* Any dimension at Low blocks the gate and attaches a specific action plan for that dimension.
* A dimension at Medium may support a conditional proceed when the condition names the evidence still owed and when it is due.

## Skill layout

* `SKILL.md`: this file (skill entrypoint).
* `templates/`: copy-ready dashboard templates.
  * `primary-dashboard.md`: the core per-dimension tracking table.
  * `status-change-view.md`: the movement-focused stakeholder update.
  * `phase-gate-summary.md`: the gate decision input.
  * `confidence-movement-log.md`: the historical change log.

> Brought to you by microsoft/hve-core
