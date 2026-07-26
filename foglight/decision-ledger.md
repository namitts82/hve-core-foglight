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

**Decision Ledger (Readiness, Risk, Impact, and Log)**

ADO Work Item: [Decision Ledger (Readiness, Risk, Impact, and Log)](https://dev.azure.com/PMaaS2022/Foglight/_workitems/edit/944)
Parent Epic: [Identifying Gaps in Existing TPM Tools, Language, and Signals](https://dev.azure.com/PMaaS2022/Foglight/_workitems/edit/903)

## Problem and Purpose

AI/ML programmes produce decisions that carry consequences far beyond the moment they are made. A decision to proceed to pilot, automate retraining, or scale infrastructure commits resources, creates dependencies, and shapes the programme's risk profile for weeks or months. In deterministic software delivery, decisions are embedded in sprint planning and release gates. In probabilistic delivery, decisions must account for what confidence exists, what is missing, what happens if the decision is wrong, how quickly failure would be detected, and whether the decision can be reversed.

Without a structured decision-management instrument, programmes encounter recurring failure modes:

* Major decisions are made in meetings without recording what evidence supported them, what was missing, or what alternatives were considered, leaving no audit trail when outcomes diverge from expectations
* Decisions are framed as binary go/no-go choices when the evidence supports conditional, staged, or reversible commitments that would reduce exposure
* The risk profile of a decision is assessed informally: teams consider what could go wrong but do not systematically evaluate detection lag (how long before failure is visible) or reversibility (how difficult it is to undo the decision)
* Decisions accumulate without a record of how each one changed the programme's trajectory, making it impossible to trace the evidence chain that led to the current state
* Irreversible decisions are treated with the same rigour as reversible ones, exposing the programme to disproportionate risk when a high-stakes commitment is made on insufficient evidence
* Post-decision impact is not tracked, so the programme cannot assess whether a decision achieved its intended effect or whether the confidence level at the time of the decision was well-calibrated

The Decision Ledger solves these problems by consolidating four decision-management functions into a single auditable artifact: Decision Readiness (what confidence exists and what is missing), Decision Risk (what happens if the decision is wrong), Decision Impact (what changed after the decision), and Decision Log (the permanent record). Decisions are treated as first-class delivery objects -- distinct from experiments, features, and tasks -- with explicit evidence requirements, risk assessments, and outcome tracking.

## Project Phases

The Decision Ledger operates across the full programme lifecycle. Decisions occur at every phase, and their character shifts from exploratory (continue or stop investigating) to operational (deploy, scale, or roll back).

::: mermaid
graph LR
  S[Scoping and Framing]
  E[Exploration]
  I[Iteration and Evaluation]
  D[Delivery and Integration]
  O[Operations]

  S --> E --> I --> D --> O

  S -.- U1[Record scoping decisions:<br>problem framing, data sources,<br>success criteria]
  E -.- U2[Record explore/pivot/stop<br>decisions after experiments]
  I -.- U3[Record phase-gate and<br>approach-commit decisions]
  D -.- U4[Record deployment,<br>rollout, and integration<br>decisions]
  O -.- U5[Record retraining, rollback,<br>and scaling decisions]

  style S fill:#FEF3C7,stroke:#F59E0B
  style E fill:#DBEAFE,stroke:#3B82F6
  style I fill:#DCFCE7,stroke:#22C55E
  style D fill:#FCE7F3,stroke:#EC4899
  style O fill:#E0E7FF,stroke:#6366F1
:::

| Phase | Decision Ledger Role |
| --- | --- |
| Scoping and Framing | Record foundational decisions: problem definition, success criteria, data source selection, and team composition. These decisions establish the constraints that all subsequent decisions operate within. |
| Exploration | Record experiment-driven decisions: continue exploring, narrow the approach set, pivot to an alternative, or stop. Each decision references the experiment evidence from the [Confidence Heatmap](https://dev.azure.com/PMaaS2022/Foglight/_workitems/edit/921). |
| Iteration and Evaluation | Record phase-gate decisions: proceed to delivery, hold for further evidence, or conditional-proceed with specific conditions. These are the highest-stakes decisions in the exploration-to-delivery transition. |
| Delivery and Integration | Record deployment decisions: infrastructure scaling, rollout strategy (canary, shadow, A/B), integration sequencing, and go-live approval. |
| Operations | Record operational decisions: retraining triggers, model rollback, monitoring threshold changes, and scaling adjustments. These decisions often have short detection windows and require rapid reversibility assessment. |

## Tool Description

The Decision Ledger is a single artifact with four sections. Each section captures a different facet of the decision lifecycle: readiness before the decision, risk at the time of the decision, impact after the decision, and the permanent log.

### Ledger Architecture

::: mermaid
graph TD
  DL[Decision Ledger]

  DR[Decision Readiness<br>What confidence exists?<br>What is missing?]
  DRK[Decision Risk<br>What happens if wrong?<br>How fast would we know?]
  DI[Decision Impact<br>What changed after<br>the decision?]
  DLOG[Decision Log<br>Permanent record<br>of all decisions]

  DL --> DR
  DL --> DRK
  DL --> DI
  DL --> DLOG

  DR -.- PRE[Before the decision]
  DRK -.- AT[At the time of<br>the decision]
  DI -.- POST[After the decision]
  DLOG -.- ALWAYS[Cumulative across<br>the lifecycle]

  style DL fill:#FDE68A,stroke:#F59E0B,stroke-width:2px
  style DR fill:#DBEAFE,stroke:#3B82F6
  style DRK fill:#FECACA,stroke:#EF4444
  style DI fill:#DCFCE7,stroke:#22C55E
  style DLOG fill:#E0E7FF,stroke:#6366F1
:::

### Section 1: Decision Readiness

Decision Readiness assesses whether the evidence base is sufficient to support the decision. It maps directly to the [Confidence Dashboard](https://dev.azure.com/PMaaS2022/Foglight/_workitems/edit/920) dimensions and forces the decision-maker to acknowledge both the evidence that exists and the evidence that is missing.

| Column | Purpose |
| --- | --- |
| Decision | The specific commitment being considered |
| Confidence Present | Which confidence dimensions are at Medium or High, with the evidence that supports them |
| Confidence Missing | Which confidence dimensions are at Low, and what evidence would be needed to raise them |
| Risk If Wrong | The consequence of proceeding without the missing confidence |
| Mitigation | The specific action that limits exposure if the decision turns out to be wrong |

### Section 2: Decision Risk

Decision Risk evaluates the risk profile of each decision along two dimensions that traditional risk registers often miss: detection lag and reversibility.

| Column | Purpose |
| --- | --- |
| Decision | The specific commitment |
| Confidence Level | The aggregate confidence state at the time of the decision |
| Risk If Wrong | The worst plausible outcome if the decision is incorrect |
| Detection Lag | How long it would take to discover that the decision was wrong (short: days, medium: weeks, long: months) |
| Reversibility | How difficult it is to undo the decision (high: easy to revert, medium: costly but possible, low: effectively irreversible) |

Detection lag and reversibility together determine the decision's true risk profile:

::: mermaid
graph TD
  D[Decision]
  DL_SHORT[Short Detection Lag<br>Days]
  DL_MEDIUM[Medium Detection Lag<br>Weeks]
  DL_LONG[Long Detection Lag<br>Months]

  D --> DL_SHORT
  D --> DL_MEDIUM
  D --> DL_LONG

  DL_SHORT --> HIGH_R[High Reversibility<br>Low overall risk]
  DL_MEDIUM --> MED_R[Medium Reversibility<br>Moderate risk]
  DL_LONG --> LOW_R[Low Reversibility<br>High overall risk]

  style D fill:#FDE68A,stroke:#F59E0B,stroke-width:2px
  style HIGH_R fill:#DCFCE7,stroke:#22C55E
  style MED_R fill:#FDE68A,stroke:#F59E0B
  style LOW_R fill:#FECACA,stroke:#EF4444
:::

* **Short detection + high reversibility:** Low-risk decisions. Monitor and revert if needed. Standard for A/B tests and canary deployments.
* **Long detection + low reversibility:** High-risk decisions. Require 🟢 High confidence in affected dimensions before proceeding. Standard for infrastructure scaling, data pipeline commitments, and production integrations.

### Section 3: Decision Impact

Decision Impact tracks what actually happened after a decision was executed. It compares the state before and after the decision and records the confidence change.

| Column | Purpose |
| --- | --- |
| Decision | The commitment that was made |
| Before | The programme state before the decision |
| After | The programme state after the decision was executed |
| Confidence Change | Which confidence dimensions moved and in which direction |

This section creates a feedback loop: the programme can assess whether its decision-making is well-calibrated by comparing the confidence level at the time of the decision with the actual outcome.

### Section 4: Decision Log

The Decision Log is the permanent, chronological record of all decisions. Every entry captures the essential facts needed to reconstruct the decision rationale at any future point.

| Column | Purpose |
| --- | --- |
| Decision | The specific commitment |
| Date | When the decision was made |
| Confidence Level | The aggregate confidence state at decision time |
| Reversibility | How easily the decision can be undone |

### Usage Rules

The following rules govern how the Decision Ledger operates:

* Every major commitment must appear in the Decision Ledger before execution
* Decisions must reference evidence and confidence, not optimism or timelines
* Irreversible decisions require 🟢 High confidence in the affected dimensions
* If a decision is not logged, it is not considered agreed
* The Decision Impact section must be updated within one learning cycle of the decision being executed
* Decisions with long detection lag and low reversibility receive mandatory TPM review before approval

### Responsibility Alignment

The Decision Ledger directly supports four Foglight responsibility areas:

| Responsibility | How the Decision Ledger Supports It |
| --- | --- |
| [Reporting and Change Control](https://dev.azure.com/PMaaS2022/Foglight/_workitems/edit/979) | The ledger structures every decision as an auditable record with evidence, confidence, risk, and impact. It creates the paper trail that reporting and change control requires, linking each decision to the evidence that supported it and the outcome it produced. |
| [Risk Management](https://dev.azure.com/PMaaS2022/Foglight/_workitems/edit/955) | The Decision Risk section introduces detection lag and reversibility as mandatory risk dimensions, preventing the programme from treating all decisions as equivalent. High-risk decisions (long detection, low reversibility) receive additional scrutiny. |
| [Stakeholder Management](https://dev.azure.com/PMaaS2022/Foglight/_workitems/edit/940) | The ledger provides stakeholders with a transparent record of what was decided, what evidence existed, and what was acknowledged as missing. This transparency builds trust and prevents post-hoc disagreements about what was agreed. |
| [Definition of Done](https://dev.azure.com/PMaaS2022/Foglight/_workitems/edit/962) | The ledger treats decisions as first-class delivery objects with explicit completion criteria: a decision is "done" when it has readiness, risk, and log entries, and its impact has been assessed after execution. |

## How to Use

### Step 1: Identify the Decision

When the programme reaches a point where a commitment must be made (proceed, stop, scale, deploy, revert), identify it as a Decision Ledger entry. Not every choice is a ledger decision. Ledger decisions involve resource commitments, direction changes, or actions that affect the programme's risk profile.

### Step 2: Complete the Readiness Assessment

Before the decision is made, populate the Decision Readiness section. Map the decision to the Confidence Dashboard dimensions. Record what evidence supports the decision and what evidence is missing. Identify the risk of proceeding without the missing evidence and the mitigation that limits exposure.

### Step 3: Complete the Risk Assessment

Evaluate the decision's detection lag and reversibility. Decisions with long detection lag and low reversibility require 🟢 High confidence. Decisions with short detection lag and high reversibility can proceed at 🟡 Medium confidence with monitoring.

### Step 4: Make and Log the Decision

Record the decision in the Decision Log with its date, confidence level, and reversibility rating. The decision is now an agreed programme commitment. Communicate it to stakeholders using the ledger entry as the source of truth.

### Step 5: Track Impact

Within one learning cycle of the decision being executed, update the Decision Impact section. Record what changed, how confidence moved, and whether the outcome matched expectations. If the outcome diverges from expectations, the impact entry becomes the evidence base for a follow-up decision (revert, adjust, or continue).

::: mermaid
flowchart TD
  START[Commitment point<br>reached] --> IDENTIFY[Identify as a<br>ledger decision]
  IDENTIFY --> READY[Complete Decision<br>Readiness assessment]
  READY --> RISK[Complete Decision<br>Risk assessment]
  RISK --> CHECK{Detection lag +<br>reversibility?}
  CHECK -- Short + High --> PROCEED[Proceed at Medium<br>confidence with monitoring]
  CHECK -- Long + Low --> GATE{All affected<br>dimensions at High?}
  GATE -- Yes --> PROCEED_HIGH[Proceed with<br>explicit mitigations]
  GATE -- No --> HOLD[Hold. Address missing<br>evidence first.]
  PROCEED --> LOG[Record in<br>Decision Log]
  PROCEED_HIGH --> LOG
  HOLD --> READY
  LOG --> EXECUTE[Execute the decision]
  EXECUTE --> IMPACT[Track impact within<br>one learning cycle]
  IMPACT --> REVIEW{Outcome matches<br>expectations?}
  REVIEW -- Yes --> DONE[Decision validated.<br>Continue.]
  REVIEW -- No --> FOLLOWUP[Create follow-up<br>decision entry]

  style START fill:#DBEAFE,stroke:#3B82F6
  style HOLD fill:#FCA5A5,stroke:#DC2626
  style DONE fill:#DCFCE7,stroke:#22C55E
  style LOG fill:#E0E7FF,stroke:#6366F1
:::

## Reusable Components

### Decision Readiness Template

| Decision | Confidence Present | Confidence Missing | Risk If Wrong | Mitigation |
| --- | --- | --- | --- | --- |
| *Specific commitment* | *Dimensions at Medium/High with evidence* | *Dimensions at Low with evidence gaps* | *Consequence of proceeding without missing evidence* | *Action that limits exposure* |

### Decision Risk Template

| Decision | Confidence Level | Risk If Wrong | Detection Lag | Reversibility |
| --- | --- | --- | --- | --- |
| *Specific commitment* | 🔴 / 🟡 / 🟢 | *Worst plausible outcome* | Short / Medium / Long | High / Medium / Low |

### Decision Impact Template

| Decision | Before | After | Confidence Change |
| --- | --- | --- | --- |
| *Commitment that was made* | *Programme state before* | *Programme state after* | *Dimension movement: ↑ / ↓ / —* |

### Decision Log Template

| Decision | Date | Confidence Level | Reversibility | Outcome Status |
| --- | --- | --- | --- | --- |
| *Specific commitment* | *Date* | 🔴 / 🟡 / 🟢 | High / Medium / Low | Pending / Validated / Reverted / Adjusted |

## Sample Use Case

### Scenario: Autonomous Vehicle Perception System for a Freight Company

A freight company is developing an ML-based perception system that detects obstacles on loading docks using camera feeds, replacing manual spotters. The programme spans 20 weeks with a safety review board that holds proceed/stop authority. The VP of Operations, the safety director, and the fleet technology officer are the primary stakeholders.

This programme carries high decision risk because:

* Obstacle detection failures can cause equipment damage or personnel injury
* The system must operate across 14 loading-dock configurations with varying lighting conditions
* False negative rates (missed obstacles) carry safety consequences; false positive rates (phantom detections) halt operations and erode driver trust
* Integration with the existing fleet management system requires a firmware update to 800 dock cameras, which is costly to reverse

### Decision 1: Proceed to Pilot (Week 8)

**Readiness:**

| Decision | Confidence Present | Confidence Missing | Risk If Wrong | Mitigation |
| --- | --- | --- | --- | --- |
| Proceed to pilot on 3 loading docks | Feasibility 🟢 (object detection achieves 96.2% recall on test set). Signal 🟢 (consistent across 5-fold cross-validation, AUROC 0.94-0.97). | Applicability 🟡 (tested on 6 of 14 dock configurations). Operational 🔴 (no firmware deployment tested, no monitoring dashboard). | Pilot fails on untested dock configurations. Camera firmware deployment causes operational disruption. | Limit pilot to 3 docks with tested configurations. Use shadow mode (system detects but does not control) for first 2 weeks. |

**Risk:**

| Decision | Confidence Level | Risk If Wrong | Detection Lag | Reversibility |
| --- | --- | --- | --- | --- |
| Proceed to pilot (shadow mode) | 🟡 Medium | Shadow mode means no safety impact. Worst case: 2 weeks of data collection on a non-functioning detector. | Short (daily monitoring of detection accuracy vs manual spotter ground truth) | High (shadow mode can be disabled instantly) |

**Log:**

| Decision | Date | Confidence Level | Reversibility | Outcome Status |
| --- | --- | --- | --- | --- |
| Proceed to pilot on 3 docks, shadow mode, 2-week evaluation | Week 8 | 🟡 Medium | High | Pending |

### Decision 2: Expand Pilot to Active Mode (Week 10)

After 2 weeks of shadow-mode pilot, the system matched or exceeded manual spotter detection on all three docks (97.1% recall vs 89% manual baseline). False positive rate: 3.2%, within the 5% target.

**Impact (from Decision 1):**

| Decision | Before | After | Confidence Change |
| --- | --- | --- | --- |
| Proceed to pilot (shadow mode) | Detection accuracy unknown in live conditions | System exceeds manual baseline on 3 dock configurations. False positive rate within target. | Applicability ↑ (Low to Medium). Signal ↑ (remains High, validated on live data). |

**Readiness (for Decision 2):**

| Decision | Confidence Present | Confidence Missing | Risk If Wrong | Mitigation |
| --- | --- | --- | --- | --- |
| Expand to active mode on 3 pilot docks | Feasibility 🟢. Signal 🟢. Applicability 🟡 (3 of 14 configurations validated). | Operational 🟡 (monitoring dashboard built but not stress-tested). Safety 🟡 (no night-shift lighting validation). | Active mode halts dock operations on false positive. Night-shift lighting may degrade detection. | Require manual spotter backup during night shifts. Set false positive auto-disable threshold at 8%. |

**Risk:**

| Decision | Confidence Level | Risk If Wrong | Detection Lag | Reversibility |
| --- | --- | --- | --- | --- |
| Expand to active mode, 3 docks, day shift only | 🟡 Medium | Unnecessary dock halts from false positives. Estimated cost: $1,200 per false halt. | Short (real-time monitoring dashboard) | High (revert to shadow mode within 1 hour) |

**Log:**

| Decision | Date | Confidence Level | Reversibility | Outcome Status |
| --- | --- | --- | --- | --- |
| Expand to active mode, 3 docks, day shift only, with manual backup at night | Week 10 | 🟡 Medium | High | Pending |

### Decision 3: Full Fleet Camera Firmware Upgrade (Week 16)

After 6 weeks of active-mode operation across 3 docks (expanded to 8 docks at week 12), the safety review board considers the firmware upgrade for all 800 dock cameras to enable the perception system fleet-wide.

**Readiness:**

| Decision | Confidence Present | Confidence Missing | Risk If Wrong | Mitigation |
| --- | --- | --- | --- | --- |
| Upgrade firmware on 800 cameras for fleet-wide deployment | Feasibility 🟢. Signal 🟢. Applicability 🟢 (12 of 14 configurations validated, night-shift validated at week 14). Operational 🟡 (monitoring dashboard stress-tested, retraining pipeline designed but untested). | Operational confidence gap: retraining pipeline not validated with production data. 2 dock configurations (cold-storage docks with condensation) untested. | Firmware upgrade is costly to reverse ($180,000 rollback cost). If detection degrades on cold-storage docks, those docks operate without perception support. | Phase the rollout: standard docks first (760 cameras), cold-storage docks (40 cameras) after a 4-week validation period. Defer retraining pipeline validation to pilot phase. |

**Risk:**

| Decision | Confidence Level | Risk If Wrong | Detection Lag | Reversibility |
| --- | --- | --- | --- | --- |
| Firmware upgrade, 760 standard cameras | 🟡 Medium (Operational at Medium) | Firmware issue affects all upgraded cameras simultaneously. Perception system downtime across fleet. | Medium (monitoring detects accuracy degradation within 48 hours, but firmware rollback requires 2 weeks of technician visits) | Low ($180,000 rollback cost, 2-week rollback window) |

This decision has **long detection lag and low reversibility** for the firmware component. The ledger's usage rules require 🟢 High confidence in affected dimensions for irreversible decisions. Operational Confidence is at 🟡 Medium.

**Safety review board decision:** Conditional proceed. Upgrade 200 cameras in the first wave (covering the 8 validated docks). Validate retraining pipeline on production data during the first wave. Full fleet upgrade contingent on Operational Confidence reaching 🟢 High.

**Log:**

| Decision | Date | Confidence Level | Reversibility | Outcome Status |
| --- | --- | --- | --- | --- |
| Firmware upgrade, wave 1: 200 cameras (8 validated docks). Full fleet contingent on Operational reaching High. | Week 16 | 🟡 Medium | Low (wave 1 scope reduces exposure) | Pending |

### What the Decision Ledger Revealed

* **Decision 1 was appropriately sized.** The ledger's reversibility assessment supported a shadow-mode pilot that generated evidence with zero safety risk. Without the ledger, the team might have proposed an active-mode pilot at week 8, when Operational Confidence was at 🔴 Low.
* **Decision 3 was correctly gated.** The ledger's rule that irreversible decisions require 🟢 High confidence prevented a premature fleet-wide firmware upgrade. The phased approach (200 cameras first) reduced the reversibility exposure while maintaining deployment momentum.
* **The impact section calibrated future decisions.** Decision 1's impact showed that live-environment performance exceeded lab performance. This evidence supported a more aggressive stance for Decision 2. Without the impact tracking, the team would have relied on lab metrics alone for every subsequent decision.

## Industry Framework Alignment

### Microsoft MLOps Maturity Model

The MLOps maturity model progresses from manual processes (Level 0) to fully automated operations (Level 4). Decision-making is implicit in the model: at Level 0, release decisions are manual and uninstrumented; at Level 4, "drift or regression signals trigger automatic retraining." The Decision Ledger makes decision-making explicit at every maturity level.

* At Level 0, the ledger provides the structured decision record that the manual process lacks. Without it, decisions at Level 0 are made in meetings and lost.
* At Level 4, automated decisions (retraining triggers, model promotion) still require auditable records. The ledger's log and impact sections serve this function for automated decision points.

The gap: the MLOps model describes how decisions are operationalised (manual vs automated) but does not prescribe how individual decisions are assessed for readiness, risk, and impact. The ledger fills this per-decision assessment gap.

### Google Rules of Machine Learning

* **Rule 9: "Detect problems before exporting models."** The Decision Ledger's readiness section enforces this at the programme level: a deployment decision must show that known problems have been detected and mitigated before the model enters production.
* **Rule 39: "Launch decisions are a proxy for long-term product goals."** This rule acknowledges that launch decisions involve multiple metrics and trade-offs. The ledger's four-section structure captures these trade-offs explicitly: readiness shows the evidence, risk shows the exposure, impact shows the outcome, and the log preserves the rationale.
* **Rule 38: "Don't waste time on new features if unaligned objectives have become the issue."** The readiness section's "Confidence Missing" column surfaces misalignment before the decision is made, preventing the team from proceeding with unaddressed strategic gaps.

The gap: Google's rules describe when and why to make decisions. They do not prescribe the structured artifact that records those decisions with enough detail to audit, learn from, and trace over time. The ledger provides that artifact.

### AWS Well-Architected Framework: Machine Learning Lens

The ML Lens's six pillars map to Decision Ledger concerns:

* **Operational Excellence and Reliability** map to detection lag and reversibility. The ledger's risk section forces explicit assessment of these dimensions before deployment decisions.
* **Security** maps to the readiness section's "Confidence Missing" column. Security gaps (data governance, access control) must be visible before a decision to proceed.
* **Cost Optimisation** maps to the impact section. Post-decision tracking reveals whether cost assumptions held.

The gap: the ML Lens prescribes architectural best practices but does not address the decision-management process that governs transitions between architecture states. The ledger provides the decision governance layer that connects architectural concerns to programme commitments.

### Summary of Alignment

| Framework | Primary Contribution | Gap the Decision Ledger Fills |
| --- | --- | --- |
| Microsoft MLOps Maturity Model | Operational capability levels for ML lifecycle automation | Per-decision assessment artifact with readiness, risk, impact, and permanent log at any maturity level |
| Google Rules of ML | Practitioner heuristics for launch decisions, problem detection, and objective alignment | Structured, auditable decision record that captures trade-offs, evidence gaps, and post-decision outcomes |
| AWS Well-Architected ML Lens | Architectural best practices across six pillars for production ML systems | Decision governance layer connecting architectural concerns to programme commitments with detection lag and reversibility assessment |

## References

* Microsoft. "MLOps maturity model." Azure Architecture Center. <https://learn.microsoft.com/azure/architecture/ai-ml/guide/mlops-maturity-model>
* Zinkevich, Martin. "Rules of Machine Learning: Best Practices for ML Engineering." Google Developers. <https://developers.google.com/machine-learning/guides/rules-of-ml>
* Google. "MLOps: Continuous delivery and automation pipelines in machine learning." Google Cloud Architecture Center. <https://cloud.google.com/architecture/mlops-continuous-delivery-and-automation-pipelines-in-machine-learning>
* AWS. "Machine Learning Lens -- AWS Well-Architected Framework." Amazon Web Services. <https://docs.aws.amazon.com/wellarchitected/latest/machine-learning-lens/machine-learning-lens.html>
* Kahneman, Daniel; Sibony, Olivier; Sunstein, Cass R. *Noise: A Flaw in Human Judgment.* Little, Brown Spark, 2021. Research on decision quality, consistency, and structured decision protocols.
