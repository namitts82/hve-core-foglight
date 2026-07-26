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

**Decision Log**

ADO Work Item: [Decision Log](https://dev.azure.com/PMaaS2022/Foglight/_workitems/edit/982)
Parent Epic: [Identifying Gaps in Existing TPM Tools, Language, and Signals](https://dev.azure.com/PMaaS2022/Foglight/_workitems/edit/903)

## Problem and Purpose

Decisions in AI/ML programs are made under conditions of evolving confidence, incomplete evidence, and shifting context. Unlike deterministic software delivery, where decisions tend to be binary (build feature X, use technology Y) and stable once made, decisions in probabilistic programs carry residual uncertainty, may need to be reversed as new evidence emerges, and depend on confidence levels that shift throughout the engagement. Without a structured record of these decisions, programs encounter a predictable set of failure modes:

* Decisions made informally in meetings or chat threads become invisible within days. When the team revisits a topic, no one can confirm what was decided, on what basis, or whether the conditions that justified the decision still hold.
* Stakeholders recall different versions of the same decision. A pivot that one stakeholder understood as "stop model B and invest in model A" may have been understood by another as "pause model B pending further data." Without a written record, alignment erodes invisibly.
* Reversed or modified decisions carry no trace of their original rationale. When a team backtracks (a normal and expected outcome in probabilistic delivery), the absence of the original decision record makes it impossible to explain why the reversal is evidence-led rather than indecisive.
* The confidence basis for a decision is lost. A decision to proceed with a pilot may have been made when Data Confidence was Medium and Feasibility Confidence was High. If conditions change two weeks later, the team cannot reconstruct whether the original decision was well-supported at the time or prematurely optimistic.
* Reversibility is never assessed at decision time. Some decisions are cheap to undo (switching an evaluation dataset), while others create hard-to-reverse commitments (signing a data licensing agreement, deploying to production). Without explicit reversibility assessment, teams treat all decisions as equally consequential or equally casual.
* Phase-gate reviews lack a decision audit trail. When a governance board asks "what decisions led to this recommendation?", the team reconstructs from memory, emails, and slide decks rather than from a single authoritative record.

The Decision Log solves this by treating every significant decision as a first-class delivery artifact with four mandatory attributes: what was decided, when, the confidence level at the time, and the reversibility of the decision. If it is not logged, it is not agreed.

## Project Phases

The Decision Log is initialised at project kickoff and accumulates entries throughout the lifecycle. Its density increases during phases where uncertainty is highest and decisions carry the greatest consequence.

::: mermaid
graph LR
  S[Scoping and Framing]
  E[Exploration]
  I[Iteration and Evaluation]
  D[Delivery and Integration]
  O[Operations]

  S --> E --> I --> D --> O

  S -.- U1[Log foundational decisions:<br>problem framing, success criteria,<br>data scope, constraints]
  E -.- U2[Log experiment-driven decisions:<br>approach selection, data strategy,<br>pivot or continue]
  I -.- U3[Log evaluation-driven decisions:<br>threshold acceptance, model selection,<br>scope adjustments]
  D -.- U4[Log delivery decisions:<br>deployment strategy, pilot scope,<br>go/no-go at gates]
  O -.- U5[Log operational decisions:<br>retraining triggers, incident<br>response, model retirement]

  style S fill:#FEF3C7,stroke:#F59E0B
  style E fill:#DBEAFE,stroke:#3B82F6
  style I fill:#DCFCE7,stroke:#22C55E
  style D fill:#FCE7F3,stroke:#EC4899
  style O fill:#E0E7FF,stroke:#6366F1
:::

| Phase | Decision Log Role |
| --- | --- |
| Scoping and Framing | Capture foundational decisions that set the programme boundary: problem definition, success metrics, data sources approved, constraints accepted, stakeholder roles, and initial scope. These entries form the baseline against which all future decisions are evaluated. |
| Exploration | Record experiment-driven decisions: which approaches to pursue, which to discard, data strategy changes, and pivot-or-continue calls at timebox boundaries. Entries in this phase carry lower confidence levels and higher reversibility by design. |
| Iteration and Evaluation | Log evaluation-driven decisions: acceptance of performance thresholds, model selection, scope adjustments based on evaluation results, and investment reallocation. Confidence levels in this phase should be climbing; entries that record declining confidence trigger review of upstream decisions. |
| Delivery and Integration | Record delivery decisions: deployment strategy, pilot scope and selection criteria, conditional gate approvals, and integration changes. Decisions here tend to carry lower reversibility and higher confidence requirements than earlier phases. |
| Operations | Capture post-deployment decisions: retraining triggers activated, incident response actions, model retirement decisions, and scope changes based on production data. Operational decisions often reference earlier log entries to maintain continuity. |

## Tool Description

The Decision Log is a chronological, append-only record of every significant decision made during an AI/ML programme. Each entry captures four mandatory attributes and a set of contextual fields that anchor the decision in evidence.

### Decision Anatomy

::: mermaid
graph TD
  DL[Decision Log Entry]

  WHAT[Decision Statement<br>What was decided,<br>stated in active voice]
  WHEN[Date<br>When the decision<br>was made]
  CONF[Confidence Level<br>The team's confidence<br>at the time of decision]
  REV[Reversibility<br>How easily this decision<br>can be unwound]
  RAT[Rationale<br>Why this option was<br>chosen over alternatives]
  EVID[Evidence Basis<br>What data, experiments,<br>or analysis informed it]
  OWNER[Decision Owner<br>Who made or approved<br>the decision]
  STATUS[Status<br>Active, Superseded,<br>or Reversed]

  DL --> WHAT
  DL --> WHEN
  DL --> CONF
  DL --> REV
  DL --> RAT
  DL --> EVID
  DL --> OWNER
  DL --> STATUS

  style DL fill:#FDE68A,stroke:#F59E0B,stroke-width:2px
  style WHAT fill:#DBEAFE,stroke:#3B82F6
  style WHEN fill:#DCFCE7,stroke:#22C55E
  style CONF fill:#FCE7F3,stroke:#EC4899
  style REV fill:#E0E7FF,stroke:#6366F1
  style RAT fill:#FEF3C7,stroke:#F59E0B
  style EVID fill:#FECACA,stroke:#EF4444
  style OWNER fill:#DCFCE7,stroke:#22C55E
  style STATUS fill:#DBEAFE,stroke:#3B82F6
:::

### Core Attributes

| Attribute | What It Captures | Why It Matters |
| --- | --- | --- |
| Decision Statement | A clear, active-voice description of what was decided. "We will proceed with XGBoost as the candidate model." Not "Model was discussed." | Eliminates ambiguity about what was actually agreed. Prevents revisionist interpretations after the fact. |
| Date | The date the decision was made or ratified. | Anchors the decision in the programme timeline. Allows correlation with evidence that was available at that point. |
| Confidence Level | The team's assessed confidence using the Foglight legend: 🔴 Low, 🟡 Medium, 🟢 High. | Records the uncertainty context at the point of decision. A decision made at Low confidence is expected to be revisited; a decision at High is expected to hold. This prevents hindsight bias when evaluating past decisions. |
| Reversibility | An explicit assessment of how easily the decision can be unwound. High (easily reversed at low cost), Medium (reversible with moderate effort or delay), Low (creates commitments that are expensive or time-consuming to undo). | Forces the team to assess commitment level before deciding. High-reversibility decisions can be made faster with less evidence. Low-reversibility decisions demand higher confidence and broader approval. |

### Contextual Fields

| Field | Purpose |
| --- | --- |
| Rationale | Explains why this option was chosen over alternatives. Captures the reasoning that might otherwise exist only in the heads of those present at the meeting. |
| Evidence Basis | Lists the specific data, experiment results, or analysis that informed the decision. Links to evaluation reports, data profiles, or stakeholder conversations. |
| Decision Owner | Identifies who made or approved the decision. Clarifies accountability and provides a contact point for future questions about the decision's context. |
| Status | Tracks the current state of the decision: Active (still in effect), Superseded (replaced by a later decision, with a cross-reference), or Reversed (explicitly undone, with the rationale for reversal). |

### The Relationship Between Confidence and Reversibility

Confidence and reversibility together determine the decision-making posture the TPM should adopt:

::: mermaid
graph TD
  MATRIX[Decision Posture Matrix]

  HH[🟢 High Confidence<br>High Reversibility<br>Decide and move on]
  HL[🟢 High Confidence<br>Low Reversibility<br>Decide with full<br>stakeholder alignment]
  LH[🔴 Low Confidence<br>High Reversibility<br>Decide quickly,<br>plan to revisit]
  LL[🔴 Low Confidence<br>Low Reversibility<br>Defer until evidence<br>improves or escalate]

  MATRIX --> HH
  MATRIX --> HL
  MATRIX --> LH
  MATRIX --> LL

  style MATRIX fill:#FDE68A,stroke:#F59E0B,stroke-width:2px
  style HH fill:#DCFCE7,stroke:#22C55E
  style HL fill:#DBEAFE,stroke:#3B82F6
  style LH fill:#FEF3C7,stroke:#F59E0B
  style LL fill:#FECACA,stroke:#EF4444
:::

| Confidence | Reversibility | Posture |
| --- | --- | --- |
| 🟢 High | High | Decide and move on. Minimal ceremony required. Log the entry for the record. |
| 🟢 High | Low | Decide with full stakeholder alignment. Record the evidence, alternatives considered, and the cost-of-change if conditions shift. |
| 🟡 Medium / 🔴 Low | High | Decide quickly and plan to revisit. Log the decision with a review date. The low cost of reversal justifies acting on incomplete information. |
| 🟡 Medium / 🔴 Low | Low | Defer until evidence improves, or escalate to a governance authority if deferral is not possible. Log the deferral itself as a decision. |

### Usage Rules

* Every decision entry must include all four mandatory attributes: statement, date, confidence, and reversibility
* The log is append-only. Decisions are never deleted. Superseded or reversed decisions remain in the log with their original rationale intact and a cross-reference to the replacement entry.
* When a decision is reversed, the reversal entry must state the new evidence or changed conditions that justify the change. This prevents perception of indecision and provides a defensible audit trail.
* Decisions deferred are logged as decisions. A deferral entry records what was under consideration, why the team chose to wait, and the conditions or evidence threshold that would trigger revisiting.
* The log is reviewed at every phase gate and at weekly learning reviews. Gate reviewers should be able to trace the decision chain that led to the current programme state by reading the log chronologically.

### Responsibility Alignment

The Decision Log directly supports three Foglight responsibility areas:

| Responsibility | How the Decision Log Supports It |
| --- | --- |
| [Reporting and Change Control](https://dev.azure.com/PMaaS2022/Foglight/_workitems/edit/979) | The log structures every status report around decisions made: what changed in the decision landscape, what new decisions were recorded, which prior decisions were revisited. Changes to scope, timeline, or approach are justified by referencing the specific decision entry and its evidence basis. Stakeholders see change as evidence-driven rather than reactive. |
| [Stakeholder Management](https://dev.azure.com/PMaaS2022/Foglight/_workitems/edit/940) | The log provides a single authoritative record that all stakeholders can reference. Alignment disputes reduce because the decision, its rationale, and its confidence level are documented at the time of agreement. When revisits are needed, the original entry prevents "that is not what we agreed" conversations. The log also allows stakeholders who were not present at a decision point to understand the reasoning without relying on second-hand accounts. |
| [Definition of Done](https://dev.azure.com/PMaaS2022/Foglight/_workitems/edit/962) | Decisions are first-class units of work in Foglight. The log tracks which decisions have been made, which are pending, and which conditions must be met before outstanding decisions can be resolved. At phase gates, done means decisions are documented, not just work completed. A sprint that produces evaluation results but does not resolve the decision those results were intended to inform has not moved the programme forward. |

## How to Use

### Step 1: Establish the Log at Kickoff

Create the Decision Log as a shared, accessible artifact during the first week of the engagement. Define the mandatory fields, agree on the confidence legend with the team, and record the first entries: the foundational decisions about problem scope, success criteria, data sources, and initial constraints. These entries form the decision baseline for the programme.

### Step 2: Log Decisions as They Happen

Record each significant decision within 24 hours of it being made. A significant decision is one that affects scope, approach, resources, timeline, or the programme's ability to proceed to the next phase. Do not batch decisions for weekly review. The value of the log depends on capturing the confidence context at the time of the decision, not in retrospect.

### Step 3: Assess Confidence and Reversibility for Every Entry

For each entry, explicitly assess and record the confidence level and reversibility. Resist the temptation to default to Medium for both. Ask the team: "If this decision turns out to be wrong, what does it cost to undo?" (reversibility) and "How strong is the evidence supporting this choice?" (confidence). The intersection of these two attributes determines the appropriate decision posture.

### Step 4: Review the Log at Phase Gates and Learning Reviews

At every phase gate, walk the governance board through the decision chain from the log. Present: (1) the major decisions made since the last gate, (2) any decisions that were reversed and why, (3) any decisions that were deferred and the conditions for resolving them, and (4) the confidence levels at the time of each decision. At weekly learning reviews, scan for decisions that were made at Low confidence and assess whether new evidence has changed the basis.

### Step 5: Mark Superseded or Reversed Decisions

When new evidence invalidates a prior decision, do not delete or edit the original entry. Create a new entry that references the original, states the new evidence, and records the reversal or replacement. Update the status of the original entry from Active to Superseded or Reversed. This preserves the audit trail and makes the reversal defensible.

::: mermaid
flowchart LR
  TRIGGER[Significant decision<br>made or pending] --> RECORD[Record decision with<br>all four mandatory<br>attributes]
  RECORD --> ASSESS{Confidence and<br>reversibility?}
  ASSESS -- High/High --> LOG[Log and proceed]
  ASSESS -- High/Low --> ALIGN[Seek stakeholder<br>alignment, then log]
  ASSESS -- Low/High --> REVISIT[Log with<br>review date]
  ASSESS -- Low/Low --> DEFER[Defer or escalate.<br>Log the deferral.]
  LOG --> GATE{Phase gate or<br>learning review?}
  ALIGN --> GATE
  REVISIT --> GATE
  DEFER --> GATE
  GATE -- Yes --> REVIEW[Walk through<br>decision chain]
  GATE -- No --> NEXT[Continue to<br>next cycle]
  REVIEW --> CHECK{Any decisions<br>need revisiting?}
  CHECK -- Yes --> UPDATE[Create new entry.<br>Mark original as<br>Superseded or Reversed.]
  CHECK -- No --> CONFIRM[Confirm decision<br>chain is current]
  UPDATE --> NEXT
  CONFIRM --> NEXT

  style TRIGGER fill:#DBEAFE,stroke:#3B82F6
  style DEFER fill:#FECACA,stroke:#EF4444
  style UPDATE fill:#FDE68A,stroke:#F59E0B
  style REVIEW fill:#E0E7FF,stroke:#6366F1
  style CONFIRM fill:#DCFCE7,stroke:#22C55E
:::

## Reusable Components

### Decision Log Table

The primary tracking artifact. One row per decision, maintained chronologically.

| # | Decision | Date | Confidence | Reversibility | Rationale | Evidence | Owner | Status |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 1 | *Active-voice description of what was decided* | *YYYY-MM-DD* | 🔴 / 🟡 / 🟢 | High / Medium / Low | *Why this option was chosen* | *Data, experiments, or analysis* | *Name or role* | Active / Superseded / Reversed |
| 2 | | | | | | | | |
| 3 | | | | | | | | |

Use this table as the running log throughout the engagement. Append entries chronologically. Do not reorder or delete rows.

### Decision Review Checklist

Use this checklist at phase gates and learning reviews to systematically assess the decision log.

| Check | Question | Finding |
| --- | --- | --- |
| Completeness | Are all significant decisions from this period recorded? | *Yes / No, with gaps identified* |
| Confidence basis | Did any decisions record 🔴 Low confidence? If so, has new evidence changed the basis? | *List of Low-confidence decisions and their current status* |
| Reversals | Were any decisions reversed since the last review? Is the reversal evidence-based and documented? | *List of reversals with references to replacement entries* |
| Deferrals | Are any deferred decisions approaching their trigger conditions? | *List of deferrals and whether conditions have been met* |
| Low-reversibility decisions | Were any Low-reversibility decisions made at less than 🟢 High confidence? Do they require escalation or additional validation? | *List of decisions and recommended actions* |
| Stakeholder alignment | Do all relevant stakeholders agree on the decisions recorded? | *Yes / No, with disagreements noted* |

### Phase Gate Decision Summary

Use this template at phase gates to present the decision chain to governance reviewers.

| # | Decision | Phase | Confidence at Decision | Reversibility | Current Status | Gate Impact |
| --- | --- | --- | --- | --- | --- | --- |
| *Ref* | *Decision statement* | *Phase name* | 🔴 / 🟡 / 🟢 | High / Medium / Low | Active / Superseded / Reversed | *How this decision affects the gate recommendation* |
| | | | | | | |
| | | | | | | |

### Stakeholder Alignment Tracker

Use this template when a decision requires broad stakeholder agreement or when disputes arise about what was decided.

| Decision # | Decision | Key Stakeholders | Alignment Status | Disagreements or Conditions | Resolution Date |
| --- | --- | --- | --- | --- | --- |
| *Ref* | *Decision statement* | *Names or roles* | Aligned / Partial / Disputed | *Nature of disagreement or conditional approval* | *YYYY-MM-DD or pending* |
| | | | | | |
| | | | | | |

## Sample Use Case

### Scenario: Patient Readmission Prediction Model for a Healthcare System

A regional healthcare system commissions an ML model to predict 30-day patient readmission risk after discharge. The model will score patients at discharge to identify those who would benefit from follow-up interventions (home nursing visits, telehealth check-ins, or pharmacist-led medication reviews). The engagement spans 14 weeks: 3 weeks of scoping, 5 weeks of exploration, 4 weeks of evaluation and iteration, and 2 weeks of deployment readiness assessment. The clinical governance board holds the proceed/stop authority at the deployment-readiness gate.

This programme involves decisions under uncertainty because:

* Patient readmission is driven by clinical, social, and behavioural factors, many of which are incompletely captured in structured data
* The model must perform acceptably across diverse patient populations (cardiac, surgical, general medicine) with different readmission baselines
* Integration with the discharge workflow affects nurses, care coordinators, and social workers
* Regulatory and clinical governance constraints shape what predictions can be acted upon and by whom

### Decision Log at End of Scoping (Week 3)

| # | Decision | Date | Confidence | Reversibility | Rationale | Evidence | Owner | Status |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 1 | Define 30-day all-cause readmission as the target outcome, excluding planned readmissions and transfers | 2026-01-12 | 🟢 High | Medium | Clinical governance confirmed this aligns with national quality reporting standards. Excluding planned readmissions avoids contaminating the signal with expected events. | National quality indicator definitions; clinical advisory board minutes | Dr. Chen (Clinical Lead) | Active |
| 2 | Use 3 years of historical discharge data from the EHR, excluding records from the oncology unit | 2026-01-14 | 🟡 Medium | High | 3-year window provides sufficient volume (approx. 42,000 discharges). Oncology excluded because its readmission patterns are clinically distinct and would require a separate model. Data profiling not yet complete. | Initial record count from EHR extract; clinical advisory recommendation | TPM | Active |
| 3 | Set model success criteria: AUROC above 0.75, calibration within 10% across all patient segments, and false-positive rate below 30% for the top-risk decile | 2026-01-16 | 🟡 Medium | High | Thresholds based on published literature comparisons and clinical governance input. May need revision once baseline performance is established. | Literature review summary; governance workshop notes | Clinical Governance Board | Active |
| 4 | Defer the decision on deployment strategy (real-time scoring at discharge vs. batch nightly scoring) until exploration demonstrates feasibility | 2026-01-16 | N/A (deferral) | N/A | Insufficient information to choose. Real-time scoring requires EHR integration that IT has not yet assessed. Nightly batch is simpler but delays intervention. Will revisit when IT architecture review is complete in week 5. | IT initial consultation | TPM | Active (deferral) |

### Decision Log at Mid-Exploration (Week 6)

| # | Decision | Date | Confidence | Reversibility | Rationale | Evidence | Owner | Status |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 5 | Proceed with gradient boosting (LightGBM) as the candidate model. Discontinue logistic regression baseline after it failed to meet the AUROC threshold on the development set. | 2026-02-03 | 🟡 Medium | High | LightGBM achieves AUROC 0.78 on 70/30 holdout; logistic regression achieves 0.71. The 7-point gap exceeds what feature engineering on the linear model could plausibly close. Model selection is easily reversible at this stage. | Experiment report EXP-003; holdout evaluation metrics | Data Science Lead | Active |
| 6 | Include social determinants of health (SDOH) features derived from census-level data linked by patient postcode. Accept the representativeness limitation that SDOH data reflects area-level averages, not individual-level conditions. | 2026-02-05 | 🟡 Medium | Medium | Adding SDOH features improved AUROC from 0.78 to 0.81 on the development set. Area-level granularity is the best available approximation. Privacy review confirmed postcode-level linkage is compliant with the organisation's data governance policy. | Experiment report EXP-005; privacy review sign-off | Data Science Lead | Active |
| 7 | Reverse decision 2: include oncology patients in the training data after profiling showed oncology readmission patterns overlap with general medicine for non-cancer causes | 2026-02-07 | 🟡 Medium | High | Data profiling revealed that 68% of oncology readmissions are non-cancer-related (infections, medication issues, deconditioning), and their feature profiles are similar to general medicine readmissions. Excluding oncology was unnecessarily reducing the training sample by 4,200 discharges. | Data profile report DP-002; clinical review of oncology readmission codes | Dr. Chen (Clinical Lead) | Active |
| 2 | *(original)* Use 3 years of historical discharge data from the EHR, excluding records from the oncology unit | 2026-01-14 | 🟡 Medium | High | | | TPM | **Reversed** (see #7) |
| 8 | Deploy as real-time scoring at discharge. IT architecture review confirmed FHIR API can support synchronous calls with acceptable latency (under 2 seconds). | 2026-02-10 | 🟢 High | Low | Resolves deferral #4. Real-time scoring enables intervention assignment during the discharge conversation rather than next-day follow-up. IT confirmed infrastructure readiness. Switching to batch after real-time development has begun would require significant rework. | IT architecture review report; FHIR API latency test results | IT Lead + TPM | Active |
| 4 | *(original)* Defer the decision on deployment strategy until exploration demonstrates feasibility | 2026-01-16 | N/A | N/A | | | TPM | **Superseded** (see #8) |

### Decision Log at Production-Readiness Gate (Week 13)

| # | Decision | Date | Confidence | Reversibility | Rationale | Evidence | Owner | Status |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 9 | Accept the final model (LightGBM v3) with AUROC 0.82, calibration error below 8% across all segments, and false-positive rate of 27% in the top-risk decile | 2026-03-03 | 🟢 High | Medium | All metrics meet or exceed the success criteria defined in decision 3. Calibration improved after recalibration on the validation set. Clinical advisory board reviewed slice-level metrics and accepted the performance profile. | Final evaluation report EVAL-007; slice-level metrics by patient segment; clinical advisory sign-off | Clinical Governance Board | Active |
| 10 | Proceed to limited pilot deployment in the general medicine and cardiac wards. Exclude surgical ward from the initial pilot due to lower model performance on surgical patients (AUROC 0.74, below the 0.75 threshold). | 2026-03-05 | 🟢 High | Medium | General medicine and cardiac wards have the highest readmission volumes and the strongest model performance. Surgical ward exclusion reduces risk while the team investigates surgical-specific features. Pilot scope is reversible (can add surgical ward later) but requires workflow changes that take time to implement. | Segment-level evaluation report; ward-level readmission volume analysis | Clinical Governance Board | Active |
| 11 | Reverse decision 3 (success criteria) for the surgical ward population only: lower the AUROC threshold to 0.72 for surgical patients during the pilot, contingent on adding surgical-specific features and re-evaluating at 6-week pilot review | 2026-03-07 | 🟡 Medium | High | Surgical readmission patterns depend on procedure type and post-operative complications that the current feature set does not capture well. Lowering the threshold temporarily allows the model to be used for surgical patients during further feature development, rather than excluding them entirely. Clinical advisory accepted the adjusted threshold for the pilot period only. | Surgical segment analysis report; clinical advisory conditional approval | Dr. Chen (Clinical Lead) | Active |
| 3 | *(original)* Set model success criteria: AUROC above 0.75, calibration within 10% across all patient segments, false-positive rate below 30% | 2026-01-16 | 🟡 Medium | High | | | Clinical Governance Board | **Superseded** (see #9 for final acceptance; see #11 for surgical ward exception) |

### What the Decision Log Revealed

Without the Decision Log, this programme would have encountered three problems that are common in clinical AI deployments:

* **The oncology reversal would have been invisible.** The team originally excluded oncology patients based on a reasonable clinical assumption. When data profiling disproved that assumption, the Decision Log made the reversal traceable and evidence-based. Entry 7 cross-references entry 2, states the new evidence, and records the clinical lead's approval. Without this record, the reversal would have appeared inconsistent to stakeholders reviewing the programme months later.
* **The deployment strategy deferral would have caused confusion.** Decision 4 (defer deployment strategy) was logged as a decision with explicit trigger conditions. When the IT architecture review completed in week 5, the team resolved the deferral with decision 8, which documented the evidence and noted the Low reversibility of committing to real-time scoring. A retrospective observer can trace the full decision chain: initial deferral, trigger condition met, resolution with evidence. Without the log, the team would have struggled to explain why they chose real-time over batch.
* **The surgical ward exception would have eroded trust.** Lowering the performance threshold for one ward could appear to a governance board as moving the goalposts. The Decision Log makes the chain explicit: the original criteria (entry 3) still apply to the primary pilot populations, the surgical exception (entry 11) is conditional on feature development and time-bounded to the pilot review, and the clinical advisory board approved the adjustment. The log prevents a perception of standard-lowering by documenting the reasoning, the evidence, and the constraints.

## Industry Framework Alignment

The Decision Log draws on concepts from established frameworks for decision governance, operational management, and production ML. This section maps the tool's design to five frameworks to show where Foglight's approach aligns, extends, or fills gaps.

### Architecture Decision Records

Architecture Decision Records (ADRs), introduced by Michael Nygard in 2011, provide a lightweight format for recording architecturally significant decisions in software projects. Each ADR is a short document with four sections: Context (the forces at play), Decision (the response to those forces), Status (proposed, accepted, deprecated, or superseded), and Consequences (the resulting context). ADRs are stored alongside the codebase and numbered sequentially, creating a permanent, chronological record.

The Decision Log adapts the ADR pattern for probabilistic delivery:

* **Chronological, append-only structure.** Both ADRs and the Decision Log use sequential, immutable records. Superseded entries are marked but never deleted, preserving the full decision history. This design prevents the "what were they thinking?" problem that Nygard identified: future team members can trace the reasoning behind any past decision.
* **Context preservation.** ADRs require a Context section that describes the forces in tension at the time of the decision. The Decision Log captures the equivalent through the Rationale and Evidence Basis fields, anchoring each decision in the information that was available when it was made.
* **Status tracking.** ADRs support proposed, accepted, deprecated, and superseded statuses. The Decision Log uses Active, Superseded, and Reversed, which captures the same lifecycle while adding an explicit Reversed status for decisions that are unwound based on new evidence.

The extension: ADRs do not capture confidence or reversibility. An ADR records what was decided and why, but not how confident the team was in the decision or how costly it would be to reverse. In deterministic software architecture, this omission is manageable because architectural decisions tend to be binary and their reversibility is implicit in the technical context. In probabilistic delivery, confidence and reversibility are first-class attributes that determine learning pace, stakeholder posture, and gate readiness. The Decision Log adds these dimensions to the ADR pattern, creating a record that communicates not just what was decided but how much weight to place on that decision and how the team should approach revisiting it.

### DACI Decision-Making Framework

The DACI framework (Driver, Approver, Contributor, Informed), widely used in product and programme management, defines clear decision-making roles to reduce ambiguity about who makes decisions and how input is gathered. According to McKinsey research cited by Atlassian, projects using DACI have a 25% higher success rate in meeting objectives and timelines compared to those without a structured decision framework.

The Decision Log complements DACI rather than replacing it:

* **DACI defines who decides. The Decision Log records what was decided.** DACI clarifies roles before the decision is made (who drives the process, who has final authority, who contributes expertise, who is informed after the fact). The Decision Log captures the outcome after the decision is made (what was decided, on what evidence, at what confidence level, with what reversibility). The two tools operate at different points in the decision lifecycle and reinforce each other.
* **The Decision Owner field maps to DACI's Approver.** Each log entry records who made or approved the decision, providing the same accountability that DACI's Approver role establishes. In practice, the DACI exercise identifies the Approver before the decision; the Decision Log records who actually exercised that authority.
* **DACI's Informed role informs log distribution.** The DACI framework identifies who should be informed after a decision. The Decision Log provides the artifact that informs them: a stakeholder in the Informed role can read the log entry to understand the decision, its rationale, and its confidence level without attending the original meeting.

The extension: DACI does not address how decisions should be revisited when conditions change. It establishes roles for making a decision but not for tracking whether that decision remains valid over time. In probabilistic programmes, decisions made at Low or Medium confidence are expected to be revisited. The Decision Log adds temporal tracking (Status field, cross-references between entries) that DACI does not prescribe, creating a mechanism for managing the full decision lifecycle from initial capture through supersession or reversal.

### Microsoft MLOps Maturity Model

Microsoft's MLOps maturity model defines five levels (0 through 4) describing an organisation's capability to develop, deploy, and operate ML systems. Each level prescribes increasing automation, monitoring, and governance. At Level 0, all processes are manual, experiments are not tracked consistently, and "the end result is typically a single model file handed off manually." By Level 4, "drift or regression signals trigger automatic retraining" and "model promotion is policy-based and automated."

The Decision Log maps to the MLOps maturity model across its governance dimensions:

* **Decision governance matures with MLOps capability.** At Level 0, decisions about model selection, data strategy, and deployment are made informally and passed between disconnected teams. The Decision Log provides a structured record that formalises these decisions regardless of automation maturity. At Level 3 and above, where CI/CD pipelines manage releases and A/B testing validates model deployment, the Decision Log captures the human decisions that automation does not replace: when to retrain, whether to accept a model version, and whether to roll back.
* **Retraining triggers are decision points.** At Level 4, drift signals can trigger automatic retraining. Each retraining event is an implicit decision: the system determined that the current model is no longer adequate and a new model should replace it. The Decision Log makes these decisions explicit by recording the trigger, the evidence (drift metric), the confidence in the replacement model, and the reversibility (rollback path). Automated decisions still need audit trails.
* **Metadata management provides the evidence basis.** The MLOps maturity model describes metadata management at Level 1 and above: tracking pipeline versions, execution parameters, model versions, and evaluation metrics. The Decision Log draws on this metadata as its Evidence Basis field. A well-maintained metadata store makes Decision Log entries more rigorous; conversely, the Decision Log identifies gaps in metadata when the evidence basis for a decision cannot be clearly cited.

The gap: the MLOps maturity model prescribes what to automate and monitor but not how to govern the human decisions that occur at every level. An organisation at MLOps Level 3 may have automated model deployment but no structured record of why a particular model version was approved for deployment. The Decision Log fills this gap by providing a programme-level decision record that complements technical metadata with human judgment, confidence assessment, and reversibility analysis.

### Google MLOps and Rules of Machine Learning

Google's MLOps framework describes three automation levels for ML pipelines, and Martin Zinkevich's Rules of Machine Learning provides 43 practitioner heuristics for managing production ML systems. Together, they define both the infrastructure and the judgment needed to operate ML at scale.

The Decision Log connects to several of these rules:

* **Rule 8: Know the freshness requirements of your system.** Freshness requirements are decisions: how often should the model retrain? What is the acceptable staleness window? The Decision Log captures these decisions with their rationale and evidence so that when freshness requirements change (because data patterns shift or usage volumes grow), the team can trace the original reasoning and assess whether the basis still holds.
* **Rules 9 and 10: Detect problems before exporting models and watch for silent failures.** When a pre-deployment check catches a problem or a monitoring system detects degradation, the response is a decision: roll back, retrain, investigate further, or accept the current state. The Decision Log records these operational decisions with the same rigour as strategic decisions, preventing the common pattern where incident responses are undocumented and unreproducible.
* **Rule 38: Don't waste time on new features if unaligned objectives have become the issue.** This rule describes a decision moment: the team must decide whether to continue feature engineering or revisit the objective. The Decision Log captures this pivot decision with its evidence (plateauing metrics, A/B test results showing user dissatisfaction despite improved model accuracy) and rationale, making the strategic shift visible and defensible.
* **Rule 39: Launch decisions are a proxy for long-term product goals.** Zinkevich observes that launch decisions depend on multiple criteria beyond model performance: daily active users, revenue, user satisfaction, and long-term product health. The Decision Log is the natural home for these multi-criteria launch decisions, capturing which metrics were weighed, what trade-offs were accepted, and at what confidence level the launch was approved.

The gap: Google's rules prescribe good engineering judgment but not how to record and communicate the outcomes of that judgment to non-technical stakeholders. The Decision Log translates engineering decisions into a structured format that programme managers, governance boards, and business stakeholders can review, trace, and audit. It bridges the gap between practitioner heuristics and stakeholder governance.

### AWS Well-Architected Framework: Machine Learning Lens

The AWS Well-Architected Framework's Machine Learning Lens applies six architectural pillars to ML workloads: Operational Excellence, Reliability, Performance Efficiency, Cost Optimisation, Security, and Sustainability. Each pillar defines best practices that require decisions throughout the ML lifecycle.

The Decision Log records the decisions that each pillar demands:

* **Operational Excellence demands decisions about team processes, monitoring, and improvement.** The Decision Log captures operational decisions: monitoring strategy, alert thresholds, retraining frequency, incident response protocols. These decisions map Operational Confidence to concrete, recorded commitments.
* **Reliability requires decisions about fault tolerance, recovery, and consistency.** Decisions about rollback strategies, fallback paths, and service level objectives are recorded with their reversibility assessed. A decision to deploy without a tested rollback path is a Low-reversibility commitment that the log makes visible.
* **Performance Efficiency requires decisions about resource allocation and model selection.** The Decision Log records trade-off decisions: choosing a smaller model for latency over a larger model for accuracy, selecting a deployment region, or setting batch sizes. These decisions carry evidence (benchmark results) and confidence levels (tested under production-like conditions or estimated from development data).
* **Cost Optimisation requires decisions about resource provisioning and scaling.** Budget commitments, compute resource selections, and cost trade-offs are decisions with financial reversibility implications. The log records these decisions so that cost overruns can be traced to specific decision points.
* **Security requires decisions about data access, encryption, and compliance.** Security decisions tend to be Low-reversibility (once data is exposed or a weaker control is accepted, the exposure cannot be undone). The Decision Log forces explicit assessment of this reversibility.
* **Sustainability requires decisions about resource efficiency.** Decisions about training frequency, model complexity, and inference infrastructure affect the system's environmental footprint. The log records these decisions alongside their rationale.

The gap: the ML Lens prescribes best practices but does not provide a decision-tracking mechanism. An organisation following the Lens may make excellent pillar-aligned decisions but have no structured record of what was decided, by whom, or at what confidence level. The Decision Log provides the tracking layer that the Lens assumes but does not prescribe.

### Summary of Alignment

| Framework | Primary Contribution | Gap the Decision Log Fills |
| --- | --- | --- |
| Architecture Decision Records | Chronological, append-only records with context, decision, status, and consequences | Adds confidence and reversibility dimensions. Adapts the pattern from deterministic architecture decisions to probabilistic delivery decisions. |
| DACI Decision-Making Framework | Clear role assignment (Driver, Approver, Contributor, Informed) for decision processes | Records decision outcomes, not just roles. Adds temporal tracking for revisiting decisions when conditions change. |
| Microsoft MLOps Maturity Model | Organisational capability levels for training, deployment, and monitoring automation | Programme-level decision governance that complements technical metadata. Makes human decisions at every maturity level auditable. |
| Google MLOps and Rules of ML | Practitioner heuristics for freshness, silent failure detection, objective alignment, and launch decisions | Structured recording of the decisions those heuristics inform, communicated in a format accessible to non-technical stakeholders. |
| AWS Well-Architected ML Lens | Architectural best practices across six pillars for production ML workloads | Decision-tracking mechanism for pillar-aligned decisions, with confidence and reversibility assessment at each decision point. |

## References

* Nygard, Michael. "Documenting Architecture Decisions." Cognitect Blog (2011). <https://cognitect.com/blog/2011/11/15/documenting-architecture-decisions>
* Atlassian. "DACI Decision-Making Framework." Atlassian Team Playbook. <https://www.atlassian.com/team-playbook/plays/daci>
* Microsoft. "MLOps maturity model." Azure Architecture Center. <https://learn.microsoft.com/azure/architecture/ai-ml/guide/mlops-maturity-model>
* Zinkevich, Martin. "Rules of Machine Learning: Best Practices for ML Engineering." Google Developers. <https://developers.google.com/machine-learning/guides/rules-of-ml>
* Google. "MLOps: Continuous delivery and automation pipelines in machine learning." Google Cloud Architecture Center. <https://cloud.google.com/architecture/mlops-continuous-delivery-and-automation-pipelines-in-machine-learning>
* AWS. "Machine Learning Lens -- AWS Well-Architected Framework." Amazon Web Services. <https://docs.aws.amazon.com/wellarchitected/latest/machine-learning-lens/machine-learning-lens.html>
