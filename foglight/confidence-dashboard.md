## Contents

1. [Tool Name](#tool-name)
2. [Problem and Purpose](#problem-and-purpose)
3. [Project Phases](#project-phases)
4. [Tool Description](#tool-description)
5. [How to Use](#how-to-use)
6. [Reusable Components](#reusable-components)
7. [Sample Use Case](#sample-use-case)
8. [Industry Framework Alignment](#industry-framework-alignment)
9. [References](#references)

## Tool Name

**Confidence Dashboard**

ADO Work Item: [Confidence Dashboard](https://dev.azure.com/PMaaS2022/Foglight/_workitems/edit/920)
Parent Epic: [Identifying Gaps in Existing TPM Tools, Language, and Signals](https://dev.azure.com/PMaaS2022/Foglight/_workitems/edit/903)

## Problem and Purpose

AI/ML programs produce progress signals that differ fundamentally from deterministic software delivery. Traditional status reports track completed tasks and remaining scope. In programs where the system's behaviour is probabilistic, tasks complete while uncertainty remains unresolved. This mismatch creates a set of failure modes that conventional progress reporting does not address:

* Teams report activity (experiments run, data collected, models trained) without connecting that activity to whether the program is approaching a decision point or stalling
* Stakeholders receive progress updates framed as green/amber/red status indicators that collapse multiple distinct uncertainties into a single dimension, hiding the specific area where confidence is lacking
* Phase-gate decisions rely on subjective readiness assessments because no structured record tracks how confidence evolved across the engagement
* Confidence that is low in one dimension (for example, data quality) masks confidence that is high in another (for example, feasibility), leading to blanket "not ready" judgments instead of targeted interventions
* Stalled confidence goes undetected because no mechanism tracks whether uncertainty is actually reducing over time; the team appears busy but the program is not converging toward a decision
* Reporting inconsistency across phases makes it difficult to compare the state of the program at different points or across different workstreams

The Confidence Dashboard solves this by decomposing program uncertainty into six named dimensions, assigning each a level based on evidence, and tracking movement over time. Confidence movement becomes the primary progress signal. Every update states the current level, what changed, why it changed, and what decision the change enables or blocks. This gives the TPM a structured, repeatable instrument for translating probabilistic outcomes into stakeholder-ready delivery signals.

## Project Phases

The Confidence Dashboard is initialised at project kickoff and updated throughout the lifecycle. Its role shifts from establishing a baseline of unknowns to tracking convergence toward production readiness.

::: mermaid
graph LR
  S[Scoping and Framing]
  E[Exploration]
  I[Iteration and Evaluation]
  D[Delivery and Integration]
  O[Operations]

  S --> E --> I --> D --> O

  S -.- U1[Initialise all six dimensions<br>at baseline confidence levels]
  E -.- U2[Track confidence movement<br>as experiments produce evidence]
  I -.- U3[Report confidence at every<br>learning cycle exit]
  D -.- U4[Present dashboard at<br>production-readiness gates]
  O -.- U5[Monitor operational confidence<br>dimensions post-deployment]

  style S fill:#FEF3C7,stroke:#F59E0B
  style E fill:#DBEAFE,stroke:#3B82F6
  style I fill:#DCFCE7,stroke:#22C55E
  style D fill:#FCE7F3,stroke:#EC4899
  style O fill:#E0E7FF,stroke:#6366F1
:::

| Phase | Confidence Dashboard Role |
| --- | --- |
| Scoping and Framing | Initialise all six dimensions. Set each to its honest starting level (typically Low or Medium). Record the evidence basis and key uncertainties for each dimension. This baseline becomes the reference point for all future updates. |
| Exploration | Update the dashboard at the end of each experiment or investigation. Track which dimensions are moving, which are stalled, and what evidence drove each change. Stalled dimensions trigger escalation or a shift in focus. |
| Iteration and Evaluation | Report the dashboard at every learning cycle exit. Stakeholder discussions centre on confidence movement rather than activity counts. Phase-gate recommendations draw directly from the dashboard state. |
| Delivery and Integration | Present the complete dashboard at production-readiness gates. All six dimensions at Medium or High with documented evidence is the minimum condition for a proceed recommendation. Any dimension at Low blocks the gate. |
| Operations | Continue tracking Operational, Signal, and Applicability confidence post-deployment. Degradation in any dimension triggers the operational response protocol (retraining, model review, or rollback). |

## Tool Description

The Confidence Dashboard organises program uncertainty into six dimensions. Each dimension captures a distinct category of risk that the TPM must track independently. The dashboard's value comes from treating these dimensions as separate signals rather than collapsing them into a single status indicator.

### Confidence Dimensions

::: mermaid
graph TD
  CD[Confidence Dashboard]

  P[Problem Confidence<br>Is the problem well-defined<br>and worth solving?]
  D[Data Confidence<br>Is the data sufficient,<br>clean, and representative?]
  F[Feasibility Confidence<br>Can a model solve this<br>problem at acceptable cost?]
  SG[Signal Confidence<br>Are model outputs stable,<br>reliable, and interpretable?]
  A[Applicability Confidence<br>Will the solution work in<br>the real operating environment?]
  O[Operational Confidence<br>Can the solution be deployed,<br>maintained, and governed?]

  CD --> P
  CD --> D
  CD --> F
  CD --> SG
  CD --> A
  CD --> O

  style CD fill:#FDE68A,stroke:#F59E0B,stroke-width:2px
  style P fill:#DBEAFE,stroke:#3B82F6
  style D fill:#DCFCE7,stroke:#22C55E
  style F fill:#FECACA,stroke:#EF4444
  style SG fill:#E0E7FF,stroke:#6366F1
  style A fill:#FCE7F3,stroke:#EC4899
  style O fill:#FEF3C7,stroke:#F59E0B
:::

| Dimension | What It Measures | Example Evidence |
| --- | --- | --- |
| Problem Confidence | Whether the problem is clearly defined, scoped, and aligned with business objectives | Stakeholder agreement on success criteria, documented problem statement, identified user need |
| Data Confidence | Whether available data is sufficient in volume, quality, coverage, and freshness for the intended use | Data profiling results, label accuracy audits, coverage analysis across required segments |
| Feasibility Confidence | Whether a model-based approach can solve the problem within the available constraints (time, cost, talent) | Baseline experiment results, proof-of-concept metrics, comparison against heuristic benchmarks |
| Signal Confidence | Whether model outputs are stable across runs, reproducible, and interpretable by downstream consumers | Metric variance across evaluation runs, inter-annotator agreement, slice-level performance consistency |
| Applicability Confidence | Whether the solution generalises to the real operating environment, including edge cases and distribution shifts | Performance on held-out production data, domain-expert review of failure cases, robustness testing |
| Operational Confidence | Whether the solution can be deployed, monitored, maintained, retrained, and governed within existing infrastructure and processes | Deployment pipeline readiness, monitoring coverage, incident response plan, compliance review |

### Confidence Levels

Each dimension is assigned one of three levels. The levels are intentionally coarse to prevent false precision. A three-level scale forces the TPM to make a clear judgment rather than hiding behind a 7-out-of-10 that masks ambiguity.

::: mermaid
graph LR
  LOW[🔴 Low<br>Significant unknowns remain.<br>Evidence is insufficient to<br>support a decision.]
  MED[🟡 Medium<br>Partial evidence exists.<br>Key risks identified but<br>not fully resolved.]
  HIGH[🟢 High<br>Strong evidence base.<br>Residual risk is understood<br>and manageable.]

  LOW --> MED --> HIGH

  style LOW fill:#FCA5A5,stroke:#DC2626
  style MED fill:#FDE68A,stroke:#F59E0B
  style HIGH fill:#DCFCE7,stroke:#22C55E
:::

| Level | Criteria | TPM Action |
| --- | --- | --- |
| 🔴 Low | Significant unknowns remain. Evidence is insufficient to support a confident decision on this dimension. | Prioritise this dimension. Direct team effort toward generating the missing evidence. Flag in stakeholder reports. |
| 🟡 Medium | Partial evidence exists. Key risks have been identified but not fully resolved. The direction is promising but not confirmed. | Monitor actively. Track whether evidence is accumulating or stalling. Plan targeted work to close remaining gaps. |
| 🟢 High | Strong evidence base. Residual risks are understood and considered manageable within defined constraints. | Maintain monitoring. Shift focus to dimensions that are still at Low or Medium. |

### Usage Rules

The following rules govern how the Confidence Dashboard operates across the engagement:

* The dashboard is initialised at project kickoff and updated at every learning cycle exit
* Confidence movement is the primary progress signal; activity without confidence movement is not progress
* Every update must state why confidence changed and what decision the change enables or blocks
* A stalled dimension (no movement across two consecutive learning cycles) triggers a mandatory review of the approach for that dimension
* The confidence legend (🔴 Low, 🟡 Medium, 🟢 High) must be included in all stakeholder-facing artifacts that reference the dashboard
* The dashboard does not replace detailed technical reports; it summarises them into a decision-oriented view

### Responsibility Alignment

The Confidence Dashboard directly supports three Foglight responsibility areas:

| Responsibility | How the Confidence Dashboard Supports It |
| --- | --- |
| [Progress Tracking](https://dev.azure.com/PMaaS2022/Foglight/_workitems/edit/960) | The dashboard makes confidence movement the primary progress metric. It tracks uncertainty reduction, hypothesis outcomes, assumption burn-down, signal stability, decision proximity, and evidence quality across six named dimensions. This replaces activity-based progress reporting with outcome-based progress reporting. |
| [Reporting and Change Control](https://dev.azure.com/PMaaS2022/Foglight/_workitems/edit/979) | The dashboard structures every status report around confidence movement, explaining what changed, why, and what it means for the next decision. Scope changes, threshold recalibrations, and priority shifts are justified by referencing the specific dimension that drove the change. |
| [Stakeholder Management](https://dev.azure.com/PMaaS2022/Foglight/_workitems/edit/940) | The dashboard provides a consistent, visual instrument that stakeholders can interpret without ML expertise. It maps the engagement landscape to six understandable dimensions, tracks sentiment and expectation alignment through confidence levels, and surfaces stalled dimensions before they become delivery crises. |

## How to Use

### Step 1: Initialise the Dashboard at Kickoff

During scoping, set each of the six dimensions to its honest starting level. For most AI/ML programs, Problem Confidence may start at Medium (the problem is identified but success criteria are not fully agreed), while Data, Feasibility, Signal, Applicability, and Operational Confidence start at Low. Record the evidence basis and key uncertainties for each dimension.

### Step 2: Update at Every Learning Cycle Exit

At the end of each timebox, sprint, or experiment cycle, review each dimension. Ask: has new evidence emerged that changes the level? If yes, update the level and record the evidence, the uncertainties that remain, and the next action. If no evidence has emerged, record that the dimension is unchanged and assess whether the current work plan is targeting the right uncertainties.

### Step 3: Escalate Stalled Dimensions

If any dimension has not moved across two consecutive learning cycles, trigger a mandatory review. Stalled confidence signals that the current approach is not generating the evidence needed to reduce uncertainty. The review should consider whether to redirect team effort, change the experimental approach, or reassess the hypothesis underlying the dimension.

### Step 4: Report Confidence Movement to Stakeholders

Present the dashboard at every stakeholder touchpoint. Lead with the Status Change View: what moved, in which direction, and why. Stakeholders should leave each update understanding which dimensions are converging and which require attention. Avoid presenting the dashboard as a static snapshot; the movement over time is the signal.

### Step 5: Gate Decisions on Dashboard State

At phase gates, use the dashboard as the primary decision input. A proceed recommendation requires all six dimensions at Medium or above. Any dimension at Low triggers a gate hold with a specific action plan to address that dimension. The dashboard record provides the evidence trail for the decision.

::: mermaid
flowchart TD
  START[End of Learning Cycle] --> REVIEW[Review each dimension<br>against new evidence]
  REVIEW --> CHANGE{Evidence changes<br>any dimension?}
  CHANGE -- Yes --> UPDATE[Update level, record<br>evidence and next action]
  CHANGE -- No --> STALL{Dimension unchanged<br>for two cycles?}
  STALL -- No --> RECORD[Record status<br>and continue]
  STALL -- Yes --> ESCALATE[Trigger mandatory<br>dimension review]
  ESCALATE --> REDIRECT[Redirect effort or<br>change approach]
  UPDATE --> REPORT[Present Status Change<br>View to stakeholders]
  RECORD --> REPORT
  REDIRECT --> REPORT
  REPORT --> GATE{Phase gate<br>approaching?}
  GATE -- Yes --> ASSESS[Assess all dimensions.<br>All Medium or above?]
  GATE -- No --> NEXT[Continue to<br>next cycle]
  ASSESS --> PROCEED{Gate decision}
  PROCEED -- All Medium+ --> GO[Proceed to<br>next phase]
  PROCEED -- Any Low --> HOLD[Gate hold.<br>Action plan for<br>Low dimensions.]

  style START fill:#DBEAFE,stroke:#3B82F6
  style ESCALATE fill:#FCA5A5,stroke:#DC2626
  style GO fill:#DCFCE7,stroke:#22C55E
  style HOLD fill:#FDE68A,stroke:#F59E0B
  style REPORT fill:#E0E7FF,stroke:#6366F1
:::

## Reusable Components

### Primary Dashboard Template

The core tracking artifact. One row per dimension, updated at every learning cycle.

| Dimension | Level | Evidence | Uncertainties | Next Action |
| --- | --- | --- | --- | --- |
| Problem Confidence | 🔴 / 🟡 / 🟢 | *What evidence supports this level* | *What remains unknown* | *What the team will do next to address this dimension* |
| Data Confidence | 🔴 / 🟡 / 🟢 | | | |
| Feasibility Confidence | 🔴 / 🟡 / 🟢 | | | |
| Signal Confidence | 🔴 / 🟡 / 🟢 | | | |
| Applicability Confidence | 🔴 / 🟡 / 🟢 | | | |
| Operational Confidence | 🔴 / 🟡 / 🟢 | | | |

### Status Change View

Use this template for stakeholder-facing status updates. The focus is on movement and its implications.

| Dimension | Confidence | Change Since Last | Why Changed | What This Enables |
| --- | --- | --- | --- | --- |
| Problem Confidence | 🔴 / 🟡 / 🟢 | ↑ / ↓ / — | *Specific evidence or event* | *Decision enabled or blocked* |
| Data Confidence | 🔴 / 🟡 / 🟢 | ↑ / ↓ / — | | |
| Feasibility Confidence | 🔴 / 🟡 / 🟢 | ↑ / ↓ / — | | |
| Signal Confidence | 🔴 / 🟡 / 🟢 | ↑ / ↓ / — | | |
| Applicability Confidence | 🔴 / 🟡 / 🟢 | ↑ / ↓ / — | | |
| Operational Confidence | 🔴 / 🟡 / 🟢 | ↑ / ↓ / — | | |

### Phase Gate Dashboard Summary

Use this template at phase gates to consolidate the dashboard state into a gate decision input.

| Dimension | Level | Trend (Last 3 Cycles) | Key Evidence | Gate Recommendation |
| --- | --- | --- | --- | --- |
| Problem Confidence | 🔴 / 🟡 / 🟢 | ↑ ↑ — | *Summary* | Proceed / Hold / Conditional |
| Data Confidence | 🔴 / 🟡 / 🟢 | | | |
| Feasibility Confidence | 🔴 / 🟡 / 🟢 | | | |
| Signal Confidence | 🔴 / 🟡 / 🟢 | | | |
| Applicability Confidence | 🔴 / 🟡 / 🟢 | | | |
| Operational Confidence | 🔴 / 🟡 / 🟢 | | | |

### Confidence Movement Log

Use this template to maintain a historical record of confidence changes for audit and retrospective purposes.

| Date | Dimension | Previous Level | New Level | Evidence for Change | Decision Enabled |
| --- | --- | --- | --- | --- | --- |
| *Date* | *Dimension name* | 🔴 / 🟡 / 🟢 | 🔴 / 🟡 / 🟢 | *What changed* | *What decision this movement supports* |

## Sample Use Case

### Scenario: Clinical Risk Prediction Model for Early Sepsis Detection

A hospital network wants to deploy an ML model that predicts sepsis onset in ICU patients 4 to 6 hours before clinical deterioration. The model will integrate with the existing electronic health record (EHR) system and generate real-time alerts for nursing staff. The engagement spans 16 weeks: 4 weeks of scoping and framing, 6 weeks of exploration and experimentation, 4 weeks of evaluation, and 2 weeks of production-readiness assessment. The clinical governance board holds the proceed/stop authority at the production-readiness gate.

This is a non-deterministic system because:

* Patient physiology varies across populations, comorbidities, and care settings
* Vital sign data arrives with variable frequency, missing values, and sensor noise
* The model must outperform the existing Modified Early Warning Score (MEWS) to justify clinical adoption
* False positive alerts risk alarm fatigue; false negatives risk patient harm
* Regulatory and clinical governance requirements introduce constraints that pure model performance does not capture

### Dashboard at Kickoff (Week 0)

| Dimension | Level | Evidence | Uncertainties | Next Action |
| --- | --- | --- | --- | --- |
| Problem Confidence | 🟡 Medium | ICU clinical lead has identified sepsis prediction as a priority. Hospital board has allocated budget for a 16-week exploratory engagement. | Success criteria not yet quantified. No agreement on acceptable false positive rate. | Workshop with clinical governance to define measurable success criteria and acceptable error thresholds. |
| Data Confidence | 🔴 Low | EHR system contains 3 years of ICU admissions data. No profiling has been done. | Unknown label quality: sepsis diagnoses may be inconsistently coded across departments. Vital sign data completeness unknown. | Conduct data profiling. Audit a sample of sepsis labels against clinical records. |
| Feasibility Confidence | 🔴 Low | Published literature reports on sepsis prediction models with AUROC 0.75 to 0.88 on similar datasets. No internal experiments conducted. | Unclear whether literature results transfer to this hospital's population and data infrastructure. | Design first experiment: replicate a published baseline approach on internal data. |
| Signal Confidence | 🔴 Low | No model exists yet. | No signals to evaluate. | Dependent on Feasibility experiments producing a first model. |
| Applicability Confidence | 🔴 Low | ICU workflow assessment not yet conducted. | Unknown how alerts will integrate with nursing handover processes. Unknown whether model performance degrades across ICU subunits (cardiac, surgical, medical). | Schedule ICU workflow observation sessions. Plan subunit-level evaluation in experiment design. |
| Operational Confidence | 🔴 Low | Hospital IT has confirmed EHR API access is technically possible. No deployment architecture defined. | No monitoring infrastructure. No retraining pipeline. Regulatory pathway (TGA Class IIa software) not yet scoped. | Engage hospital IT for architecture review. Consult regulatory team on classification pathway. |

### Dashboard at Mid-Exploration (Week 7)

| Dimension | Confidence | Change Since Last | Why Changed | What This Enables |
| --- | --- | --- | --- | --- |
| Problem Confidence | 🟢 High | ↑ from Medium | Clinical governance workshop completed. Agreed success criteria: AUROC greater than 0.80, false positive rate below 15%, alert lead time of 4+ hours. Board signed off on criteria document. | Success criteria are now the benchmark for all subsequent evaluations. No ambiguity about what "good enough" means. |
| Data Confidence | 🟡 Medium | ↑ from Low | Data profiling complete: 12,400 ICU admissions over 3 years. 78% have complete vital sign records. Label audit found 89% agreement between ICD codes and chart review on a 200-patient sample. | Data is sufficient for initial model training. Remaining 11% label discrepancy requires a labelling reconciliation plan before production use. |
| Feasibility Confidence | 🟡 Medium | ↑ from Low | Baseline XGBoost model achieves AUROC 0.82 on 70/30 holdout split. Outperforms MEWS baseline (AUROC 0.68) by 14 points. False positive rate at current threshold is 22%, above the 15% target. | Model feasibility is demonstrated. The remaining challenge is threshold tuning to reduce false positives without sacrificing sensitivity. |
| Signal Confidence | 🟡 Medium | ↑ from Low | Three consecutive training runs produce AUROC within 0.80 to 0.84. Slice-level analysis shows consistent performance across medical and surgical ICU subunits. Cardiac ICU is 6 points lower (AUROC 0.76). | Model outputs are reproducible. Cardiac ICU performance gap needs targeted investigation. |
| Applicability Confidence | 🔴 Low | — (unchanged) | ICU workflow observations scheduled but not yet completed. No real-world integration testing. | Stalled for two cycles. Triggers mandatory review. Must prioritise workflow observation in weeks 8 to 9. |
| Operational Confidence | 🟡 Medium | ↑ from Low | Hospital IT completed architecture review. EHR integration via FHIR API confirmed. Monitoring dashboard prototype built. Regulatory team confirmed TGA Class IIa pathway with 8-week approval timeline. | Deployment pathway is scoped. Regulatory timeline is factored into the schedule. Retraining pipeline design can proceed. |

**Escalation triggered:** Applicability Confidence has not moved in two consecutive cycles. Mandatory review scheduled for week 8 to redirect effort toward ICU workflow integration assessment.

### Dashboard at Production-Readiness Gate (Week 14)

| Dimension | Level | Trend (Last 3 Cycles) | Key Evidence | Gate Recommendation |
| --- | --- | --- | --- | --- |
| Problem Confidence | 🟢 High | — — — | Success criteria agreed and unchanged since week 3. All evaluation metrics reference these criteria. | Proceed |
| Data Confidence | 🟢 High | — ↑ — | Label reconciliation completed in week 10. Agreement rate raised to 95% on reconciled dataset. Data pipeline validated for production ingestion. | Proceed |
| Feasibility Confidence | 🟢 High | ↑ ↑ — | Final model (LightGBM ensemble) achieves AUROC 0.86, false positive rate 13.8%, alert lead time median 4.8 hours. All metrics within agreed success criteria. | Proceed |
| Signal Confidence | 🟢 High | ↑ — — | Five-fold cross-validation AUROC range: 0.84 to 0.88. Cardiac ICU gap closed to 2 points after feature engineering for cardiac-specific vitals. | Proceed |
| Applicability Confidence | 🟡 Medium | ↑ ↑ — | Workflow observation completed at week 9. Alert delivery redesigned to integrate with nursing handover checklist. Pilot test with 3 nurses over 2 weeks showed 91% of alerts reviewed within 10 minutes. Two edge cases identified: simultaneous multi-patient alerts and night-shift staffing. | Conditional: proceed to limited pilot with mitigation plan for identified edge cases |
| Operational Confidence | 🟡 Medium | ↑ — — | FHIR integration tested end-to-end. Monitoring dashboard tracks prediction volume, alert response time, and model drift. Retraining pipeline designed but not yet tested with production data. TGA submission prepared. | Conditional: proceed contingent on retraining pipeline validation during pilot |

**Gate decision: Proceed to limited pilot** in the medical ICU (highest data quality, most established workflow) with two conditions: (1) develop a protocol for multi-patient concurrent alerts and (2) validate the retraining pipeline on production data during the pilot. Full rollout to surgical and cardiac ICUs contingent on pilot results and TGA approval.

### What the Confidence Dashboard Revealed

Without the Confidence Dashboard, this program would have faced three risks that are common in clinical AI deployments:

* **Applicability would have surfaced at production.** The dashboard's stall detection at week 7 forced the team to prioritise ICU workflow integration earlier than planned. Without that signal, the team would have continued optimising model metrics and discovered the workflow integration challenges at the production gate, causing a multi-week delay and possible loss of clinical stakeholder trust.
* **The cardiac ICU gap would have been hidden.** Traditional progress reporting would have shown an aggregate AUROC of 0.82 without revealing the 6-point performance gap in the cardiac subunit. The dashboard's dimension-level tracking made this gap visible at mid-exploration, giving the team time to develop cardiac-specific features that closed the gap before the gate.
* **The gate decision would have been binary.** Without the dashboard, the clinical governance board would have faced a simple "ready or not ready" decision. The dashboard enabled a conditional proceed with specific, evidence-backed conditions attached to the two dimensions at Medium. The board understood exactly what remained uncertain and agreed to a phased rollout that addressed those uncertainties during the pilot.

## Industry Framework Alignment

The Confidence Dashboard draws on concepts that appear across established frameworks for managing technology maturity, operational performance, and production ML systems. This section maps the tool's design to five frameworks to show where Foglight's approach aligns, extends, or fills gaps.

### NASA Technology Readiness Levels

NASA's Technology Readiness Level (TRL) scale rates technology maturity from 1 (basic principles observed) to 9 (actual system proven in operational environment). Developed in the 1970s and formalised in 1989, TRLs provide a common vocabulary for assessing whether a technology is ready to transition from research to production. The US Department of Defence, the European Space Agency, and the European Commission all adopted the scale for procurement and research programme governance.

The Confidence Dashboard aligns with TRLs in several ways:

* **Maturity as a progression through evidence gates.** TRL advancement requires demonstrated evidence at each level: proof of concept at TRL 3, validated component at TRL 4, prototype demonstration at TRL 6, qualified system at TRL 8. The Confidence Dashboard applies the same logic to AI/ML programs: each dimension progresses from Low to Medium to High only when evidence justifies the transition. The dashboard's emphasis on evidence over activity mirrors the TRL requirement that advancement must be demonstrated, not claimed.
* **Multi-dimensional assessment.** TRLs compress maturity into a single number. A system at TRL 6 may have well-validated algorithms but untested operational support, or proven components but unresolved integration challenges. The Confidence Dashboard decomposes this single scale into six dimensions, each assessed independently. This prevents the situation where high feasibility confidence masks low operational confidence, a failure mode that TRL's single-axis design permits.
* **Decision-gate integration.** TRL assessments are conducted at programme milestones to support funding and transition decisions. The Confidence Dashboard serves the same function at phase gates: the dashboard state is the primary input to the proceed/hold decision. The difference is cadence: TRL reviews typically occur at major programme milestones, while the dashboard updates at every learning cycle, providing earlier warning of maturity stalls.

The gap: TRLs measure what has been achieved against a fixed nine-level scale. They do not track the rate of progress or detect stalled maturity. A programme can sit at TRL 4 indefinitely without the scale itself signalling a problem. The Confidence Dashboard adds temporal tracking: stalled dimensions (no movement across two consecutive cycles) trigger mandatory reviews. This stall detection mechanism has no equivalent in the TRL framework.

### Balanced Scorecard

The Balanced Scorecard, proposed by Robert Kaplan and David Norton in 1992, provides a multi-perspective performance measurement framework. The original design organises metrics across four perspectives: Financial, Customer, Internal Business Processes, and Learning and Growth. The core insight is that financial measures alone do not capture the full picture of organisational performance, and that managers need a balanced set of indicators that connect strategic objectives to operational activity.

The Confidence Dashboard applies the Balanced Scorecard's structural principles to AI/ML programme delivery:

* **Multiple perspectives as independent dimensions.** The Balanced Scorecard rejects single-metric performance tracking in favour of a small number of measures distributed across distinct perspectives. The Confidence Dashboard follows the same principle: six dimensions, each measuring a different facet of programme readiness. A programme that scores well on Feasibility but poorly on Applicability is not "75% ready"; it has a specific gap that requires targeted action.
* **Strategy translation.** The Balanced Scorecard's design process requires managers to translate strategic objectives into specific, measurable indicators. The Confidence Dashboard requires the TPM to translate programme uncertainty into specific dimensions with defined evidence criteria and level thresholds. Both tools convert abstract goals into actionable measurement structures.
* **Leading and lagging indicators.** The Balanced Scorecard distinguishes between lagging indicators (outcomes already achieved) and leading indicators (drivers of future outcomes). The Confidence Dashboard's dimensions function as leading indicators for the ultimate lagging indicator: successful production deployment. Problem and Data Confidence are early-stage leading indicators; Signal and Applicability Confidence are mid-stage; Operational Confidence is a late-stage leading indicator that predicts post-deployment success.

The gap: the Balanced Scorecard was designed for ongoing organisational performance management. It tracks steady-state metrics against targets. It does not address the specific challenge of programmes where the metrics themselves are uncertain and evolving. The Confidence Dashboard addresses this by defining levels (Low, Medium, High) rather than fixed targets, allowing the measurement framework to accommodate the inherent uncertainty of AI/ML exploration. The dashboard also adds a temporal stall-detection mechanism that the Balanced Scorecard does not prescribe.

### Microsoft MLOps Maturity Model

Microsoft's MLOps maturity model defines five levels (0 through 4) describing an organisation's capability to develop, deploy, and operate ML systems. At Level 0, all processes are manual. At Level 4, automated pipelines handle training, validation, deployment, and monitoring, with "drift or regression signals triggering automatic retraining." The model traces a progression from ad-hoc experimentation to industrialised ML operations.

The Confidence Dashboard maps to the MLOps maturity model across its monitoring and decision dimensions:

* **Data Confidence aligns with data management maturity.** At MLOps Level 0, data gathering is manual and unvalidated. At Level 3 and above, data pipelines include automated validation, schema enforcement, and drift detection. The dashboard's Data Confidence dimension tracks this progression for a specific programme: Low when data quality is unassessed, Medium when profiling is complete but gaps remain, High when data pipelines are validated and monitored.
* **Signal and Feasibility Confidence align with model validation maturity.** At Level 1, basic model testing exists. At Level 2, automated training pipelines produce reproducible results. At Level 4, deployed models emit centralised metrics and automated evaluation triggers retraining. The dashboard's Signal Confidence dimension tracks whether model outputs are stable and reproducible; Feasibility Confidence tracks whether the model meets its performance targets. Together, these dimensions capture the same concerns that the MLOps model addresses through automation maturity.
* **Operational Confidence aligns with deployment and monitoring maturity.** At Level 3, automated deployment with A/B testing is in place. At Level 4, the system monitors for drift, staleness, and performance degradation. Operational Confidence tracks the programme's readiness for this operational state: Low when no deployment architecture exists, Medium when infrastructure is scoped, High when deployment, monitoring, and retraining pipelines are validated.

The gap: the MLOps maturity model describes organisational capabilities, not programme-level decision readiness. An organisation at MLOps Level 3 may still have a specific programme where Data Confidence is Low because the data for that use case has not been profiled. The Confidence Dashboard fills this gap by providing a programme-level assessment instrument that tracks readiness for a specific initiative, regardless of the organisation's overall maturity. It also adds Problem and Applicability dimensions that the MLOps model does not address, since the MLOps model assumes the problem is defined and the environment is known.

### Google MLOps and Rules of Machine Learning

Google's MLOps framework describes three automation levels (0, 1, and 2) for ML pipelines, progressing from manual experimentation to fully automated CI/CD systems. Martin Zinkevich's *Rules of Machine Learning* provides 43 practical rules distilled from Google's production ML experience. Together, these resources define both the operational infrastructure and the practitioner judgment needed to manage ML systems through their lifecycle.

The Confidence Dashboard draws on concepts from both:

* **Rule 2: "First, design and implement metrics."** This rule establishes that measurement infrastructure must precede model development. The Confidence Dashboard operationalises this for programme management: the six dimensions and their evidence criteria are defined at kickoff before experimentation begins. Confidence levels cannot be assessed without agreed metrics, and the dashboard makes this dependency explicit.
* **Rules 8 through 11: Silent failures, freshness, and staleness.** Rule 8 (know the freshness requirements of your system), Rule 9 (detect problems before exporting a model), and Rule 10 (watch for silent failures) describe operational risks that erode model reliability without visible warning. The dashboard's Signal Confidence and Operational Confidence dimensions track these risks: stalled Signal Confidence may indicate a silent failure, and declining Operational Confidence may indicate freshness or monitoring gaps.
* **Rule 38: "Don't waste time on new features if unaligned objectives have become the issue."** This rule identifies the failure mode where teams continue technical work when the problem is strategic misalignment. The dashboard's Problem Confidence dimension captures this: if Problem Confidence drops while other dimensions progress, the TPM has a signal that the problem framing needs revisiting, not the model architecture.
* **MLOps Level 2: Automated model validation.** At Level 2, the pipeline includes automated data validation (schema skews, data value skews) and model validation (evaluation metrics, segment-level consistency). The dashboard's mid-exploration snapshot maps to these concepts: Data Confidence reflects data validation status, and Signal Confidence reflects model validation results.

The gap: Google's rules are practitioner guidance for ML engineers. They describe what to watch for but do not prescribe how to communicate findings to non-technical stakeholders or how to structure programme-level decisions around those findings. The Confidence Dashboard translates practitioner observations into a six-dimension framework that stakeholders can interpret, track, and act on. It converts "Rule 10: watch for silent failures" into a structured signal (stalled Signal Confidence) with a mandated team response (mandatory dimension review).

### AWS Well-Architected Framework: Machine Learning Lens

The AWS Well-Architected Framework's Machine Learning Lens applies six architectural pillars to ML workloads: Operational Excellence, Reliability, Performance Efficiency, Cost Optimisation, Security, and Sustainability. Each pillar defines best practices for designing, deploying, and operating ML systems in production.

The Confidence Dashboard maps each pillar to one or more confidence dimensions:

* **Operational Excellence maps to Operational Confidence.** The pillar emphasises automation, monitoring, and continuous improvement of ML operations. Operational Confidence tracks whether the programme has achieved the deployment, monitoring, and governance readiness that this pillar demands.
* **Reliability maps to Signal Confidence and Operational Confidence.** The pillar focuses on system resilience, recovery, and consistent behaviour under varying conditions. Signal Confidence tracks whether model outputs are stable and reproducible; Operational Confidence tracks whether the infrastructure supports reliable operation.
* **Performance Efficiency maps to Applicability Confidence.** The pillar addresses the efficient use of computing resources to meet requirements across changing demand patterns. Applicability Confidence tracks whether the model performs effectively in the real operating environment, including edge cases and distribution shifts that affect computational efficiency.
* **Cost Optimisation maps to Operational Confidence.** The pillar focuses on delivering business value at the lowest price point. The dashboard tracks cost-related constraints through Operational Confidence (whether the solution can be maintained within budget) and complements this with Kill Criteria cost thresholds for specific boundaries.
* **Security maps to Data Confidence.** The pillar addresses data protection, access control, and compliance. Data Confidence encompasses whether data meets quality, governance, and security requirements for the intended use.
* **Sustainability maps to Operational Confidence.** The pillar focuses on minimising environmental impact and maximising efficiency of provisioned resources. Operational Confidence tracks whether the operational footprint is sustainable within defined constraints.

The gap: the ML Lens prescribes architectural best practices for production systems. It does not provide a programme-level decision framework for the exploration-to-production journey. The Confidence Dashboard adds a decision-readiness framing that the Lens does not prescribe: it tracks when each pillar's concerns have been sufficiently addressed to justify moving to the next phase. It also adds Problem Confidence and Feasibility Confidence, dimensions that the ML Lens assumes are resolved before architectural design begins.

### Summary of Alignment

| Framework | Primary Contribution | Gap the Confidence Dashboard Fills |
| --- | --- | --- |
| NASA TRL | Maturity progression through evidence-gated levels, common vocabulary for technology readiness | Multi-dimensional assessment (six dimensions vs one scale), temporal stall detection, learning-cycle-level cadence |
| Balanced Scorecard | Multi-perspective performance measurement, strategy translation into measurable indicators | Adaptation for uncertain and evolving metrics, stall detection, uncertainty-native level definitions |
| Microsoft MLOps Maturity Model | Organisational capability levels for ML operations, automated monitoring and retraining triggers | Programme-level readiness assessment for a specific initiative, Problem and Applicability dimensions, stakeholder decision framing |
| Google MLOps and Rules of ML | Practitioner heuristics for data validation, model validation, silent failure detection, and pipeline automation | Translation of engineering signals into stakeholder-readable dimensions with mandated escalation protocols |
| AWS Well-Architected ML Lens | Architectural best practices across six pillars for production ML systems | Decision-readiness framing for the exploration-to-production journey, Problem and Feasibility dimensions, phase-gate integration |

## References

* NASA. "Technology Readiness Level Definitions." NASA. <https://www.nasa.gov/pdf/458490main_TRL_Definitions.pdf>
* Mihaly, Heder. "From NASA to EU: the evolution of the TRL scale in Public Sector Innovation." *The Innovation Journal* 22 (2017): 1-23. Summarised in "Technology readiness level." Wikipedia. <https://en.wikipedia.org/wiki/Technology_readiness_level>
* Kaplan, Robert S.; Norton, David P. "The Balanced Scorecard -- Measures that Drive Performance." *Harvard Business Review* (January-February 1992). Summarised in "Balanced scorecard." Wikipedia. <https://en.wikipedia.org/wiki/Balanced_scorecard>
* Microsoft. "MLOps maturity model." Azure Architecture Center. <https://learn.microsoft.com/azure/architecture/ai-ml/guide/mlops-maturity-model>
* Zinkevich, Martin. "Rules of Machine Learning: Best Practices for ML Engineering." Google Developers. <https://developers.google.com/machine-learning/guides/rules-of-ml>
* Google. "MLOps: Continuous delivery and automation pipelines in machine learning." Google Cloud Architecture Center. <https://cloud.google.com/architecture/mlops-continuous-delivery-and-automation-pipelines-in-machine-learning>
* AWS. "Machine Learning Lens -- AWS Well-Architected Framework." Amazon Web Services. <https://docs.aws.amazon.com/wellarchitected/latest/machine-learning-lens/machine-learning-lens.html>
