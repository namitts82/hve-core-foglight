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

**Assumption Register**

ADO Work Item: [Assumption Register](https://dev.azure.com/PMaaS2022/Foglight/_workitems/edit/963)
Parent Epic: [Identifying Gaps in Existing TPM Tools, Language, and Signals](https://dev.azure.com/PMaaS2022/Foglight/_workitems/edit/903)

## Problem and Purpose

Every AI/ML programme begins with assumptions: the labels are reliable, the data is representative, the chosen metric captures business value, latency constraints are achievable. These assumptions are often implicit -- embedded in planning documents, sprint backlogs, and team conversations without ever being stated, tested, or tracked. When an assumption fails, the programme discovers it through a downstream consequence (the model underperforms, the integration breaks, the stakeholder rejects the output) rather than through a deliberate test.

Without an explicit assumption register, programmes encounter these failure modes:

* Assumptions that underpin critical decisions are never stated, so they cannot be deliberately tested or monitored
* Work items are marked "done" based on task completion without assessing whether the assumptions that made the task meaningful still hold
* Invalidated assumptions persist in the programme's mental model because no mechanism forces the team to confront and retire them
* The same assumption is tested incidentally by multiple work streams without anyone recognising the overlap or consolidating the evidence
* Progress is overstated because the team counts completed tasks without accounting for the assumptions that remain untested beneath them
* Retrospective analysis of a failed approach cannot trace which specific assumption failed because assumptions were never recorded

The Assumption Register makes assumptions explicit, assigns them owners, tracks their status as evidence accumulates, and enforces a rule: a work item cannot be marked "done" unless it updates the register. This converts assumptions from invisible preconditions into tracked, testable objects that the programme must resolve.

## Project Phases

::: mermaid
graph LR
  S[Scoping and Framing]
  E[Exploration]
  I[Iteration and Evaluation]
  D[Delivery and Integration]
  O[Operations]

  S --> E --> I --> D --> O

  S -.- U1[Identify and record<br>initial assumptions]
  E -.- U2[Test assumptions through<br>experiments. Update status.]
  I -.- U3[Validate remaining<br>assumptions against<br>production-like conditions]
  D -.- U4[Confirm operational<br>assumptions before<br>deployment]
  O -.- U5[Monitor assumption<br>validity post-deployment]

  style S fill:#FEF3C7,stroke:#F59E0B
  style E fill:#DBEAFE,stroke:#3B82F6
  style I fill:#DCFCE7,stroke:#22C55E
  style D fill:#FCE7F3,stroke:#EC4899
  style O fill:#E0E7FF,stroke:#6366F1
:::

| Phase | Assumption Register Role |
| --- | --- |
| Scoping and Framing | Identify and record the programme's foundational assumptions: data quality, problem feasibility, resource availability, stakeholder alignment. |
| Exploration | Test assumptions through experiments. Update status from Untested to Valid, Weakened, or Killed based on evidence. |
| Iteration and Evaluation | Validate remaining assumptions against production-like conditions. Assumptions that hold in the lab but fail under realistic conditions are flagged. |
| Delivery and Integration | Confirm that operational assumptions (monitoring, retraining, rollback) are valid before deployment. |
| Operations | Monitor assumption validity post-deployment. Data assumptions, performance assumptions, and cost assumptions may degrade over time. |

## Tool Description

### Register Structure

Each assumption is a row in the register with five columns:

::: mermaid
graph TD
  AR[Assumption Register]

  A[Assumption<br>The specific belief<br>being tracked]
  T[Type<br>Data / Model / Ops /<br>Business / Integration]
  O[Owner<br>Who is responsible<br>for testing it]
  S[Status<br>Untested / Valid /<br>Weakened / Killed]
  E[Evidence<br>What supports the<br>current status]

  AR --> A
  AR --> T
  AR --> O
  AR --> S
  AR --> E

  style AR fill:#FDE68A,stroke:#F59E0B,stroke-width:2px
  style A fill:#DBEAFE,stroke:#3B82F6
  style T fill:#DCFCE7,stroke:#22C55E
  style O fill:#FCE7F3,stroke:#EC4899
  style S fill:#FECACA,stroke:#EF4444
  style E fill:#E0E7FF,stroke:#6366F1
:::

| Column | Purpose |
| --- | --- |
| Assumption | The specific, testable belief. Must be stated precisely enough that evidence can confirm or invalidate it. |
| Type | Category: Data, Model, Ops, Business, Integration. Helps prioritise which assumptions to test first based on the current phase. |
| Owner | The person responsible for testing the assumption and updating its status. |
| Status | 🟢 Valid (confirmed by evidence), 🟡 Weakened (partial evidence contradicts it), 🔴 Killed (evidence invalidates it), ⬜ Untested (no evidence yet). |
| Evidence | The specific experiment result, data finding, or observation that supports the current status. |

### The DoD Rule

**A work item cannot be marked "done" unless it updates the Assumption Register.** Every completed task should either test an assumption (and update its status) or identify a new assumption (and add it to the register). Work that neither tests nor discovers assumptions is disconnected from the programme's evidence trail.

### Assumption Burn-Down

The register enables an assumption burn-down metric: the count of Untested assumptions decreases over time as the programme generates evidence. A healthy programme shows assumptions moving from ⬜ Untested to 🟢 Valid or 🔴 Killed. A stalled programme shows Untested assumptions persisting across multiple cycles.

### Responsibility Alignment

| Responsibility | How the Assumption Register Supports It |
| --- | --- |
| [Definition of Done](https://dev.azure.com/PMaaS2022/Foglight/_workitems/edit/962) | The DoD rule ties task completion to assumption resolution. A sprint is not done unless assumptions were tested. A phase is not done unless critical assumptions are resolved. |
| [Progress Tracking](https://dev.azure.com/PMaaS2022/Foglight/_workitems/edit/960) | Assumption burn-down provides a leading progress indicator: the rate at which the programme is resolving its foundational beliefs. High Untested counts late in the programme signal hidden risk. |
| [Risk Management](https://dev.azure.com/PMaaS2022/Foglight/_workitems/edit/955) | Weakened and Killed assumptions are risk events. A Killed assumption triggers a [Backtracking Narrative](https://dev.azure.com/PMaaS2022/Foglight/_workitems/edit/943) and may require a direction change. Weakened assumptions are monitored for further degradation. |

## How to Use

### Step 1: Populate at Kickoff

During scoping, identify all assumptions the programme depends on. State each precisely. Assign a type and an owner. Set all initial statuses to ⬜ Untested.

### Step 2: Update at Every Cycle Boundary

At every [Timeboxed Learning Cycle](https://dev.azure.com/PMaaS2022/Foglight/_workitems/edit/946) boundary, review the register. Update statuses based on the cycle's evidence. Add newly discovered assumptions.

### Step 3: Enforce the DoD Rule

During sprint reviews, verify that completed work items reference assumption updates. Reject "done" claims that do not connect to the register.

### Step 4: Track the Burn-Down

Report the assumption burn-down at stakeholder updates: how many assumptions are Untested, Valid, Weakened, and Killed. Trend the counts over time.

::: mermaid
flowchart TD
  START[Populate register<br>at kickoff] --> CYCLE[Execute learning cycle]
  CYCLE --> BOUNDARY[Cycle boundary]
  BOUNDARY --> UPDATE[Update assumption<br>statuses with evidence]
  UPDATE --> BURNDOWN{Untested count<br>decreasing?}
  BURNDOWN -- Yes --> REPORT[Report burn-down<br>to stakeholders]
  BURNDOWN -- No --> DIAGNOSE[Diagnose: why are<br>assumptions untested?]
  DIAGNOSE --> REPORT
  REPORT --> DOD[Enforce DoD rule<br>on work items]
  DOD --> CYCLE

  style START fill:#DBEAFE,stroke:#3B82F6
  style DIAGNOSE fill:#FDE68A,stroke:#F59E0B
  style REPORT fill:#DCFCE7,stroke:#22C55E
:::

## Reusable Components

### Assumption Register Template

| Assumption | Type | Owner | Status | Evidence |
| --- | --- | --- | --- | --- |
| *Specific testable belief* | Data / Model / Ops / Business / Integration | *Name* | ⬜ / 🟢 / 🟡 / 🔴 | *What supports this status* |

### Assumption Burn-Down Tracker

| Cycle | Untested | Valid | Weakened | Killed | Total |
| --- | --- | --- | --- | --- | --- |
| Kickoff | *N* | 0 | 0 | 0 | *N* |
| Cycle 1 | | | | | |
| Cycle 2 | | | | | |

## Sample Use Case

### Scenario: Recommendation Engine for a Streaming Platform

A streaming platform is building a personalised content recommendation engine. The programme spans 14 weeks.

### Register at Kickoff

| Assumption | Type | Owner | Status | Evidence |
| --- | --- | --- | --- | --- |
| User viewing history is a reliable signal for preference | Data | DS Lead | ⬜ Untested | None |
| Collaborative filtering will outperform content-based baseline | Model | ML Engineer | ⬜ Untested | None |
| API latency under 100ms is achievable at peak load | Ops | Platform Eng | ⬜ Untested | None |
| Users perceive recommendations as relevant when CTR > 3% | Business | Product Manager | ⬜ Untested | None |
| Historical data from the last 12 months is representative | Data | Data Engineer | ⬜ Untested | None |

### Register at End of Cycle 2 (Week 8)

| Assumption | Type | Owner | Status | Evidence |
| --- | --- | --- | --- | --- |
| User viewing history is a reliable signal for preference | Data | DS Lead | 🟢 Valid | Correlation analysis: viewing history predicts next-watch with 0.64 Spearman correlation. Stronger than search history (0.41). |
| Collaborative filtering will outperform content-based baseline | Model | ML Engineer | 🟡 Weakened | CF achieves 4.1% CTR vs 3.8% content-based on holdout. Difference is marginal and not statistically significant at p<0.05. Hybrid approach may be needed. |
| API latency under 100ms is achievable at peak load | Ops | Platform Eng | 🔴 Killed | Load test at 10x average traffic: p99 latency 142ms. Model inference is the bottleneck. Quantisation or model distillation required. |
| Users perceive recommendations as relevant when CTR > 3% | Business | Product Manager | 🟢 Valid | A/B test with the baseline model: 3.8% CTR correlates with positive user satisfaction survey scores (4.2/5). |
| Historical data from the last 12 months is representative | Data | Data Engineer | 🟡 Weakened | Content catalogue changed 30% in the last 3 months due to new licensing deals. Older viewing patterns for removed content may not transfer. |

**Burn-down:** 5 Untested at kickoff, 0 Untested at Cycle 2. 2 Valid, 2 Weakened, 1 Killed. The Killed assumption (latency) triggered a [Backtracking Narrative](https://dev.azure.com/PMaaS2022/Foglight/_workitems/edit/943) and a pivot to model quantisation.

## Industry Framework Alignment

### Microsoft MLOps Maturity Model

At Level 0, assumptions are implicit and untested. The register provides the structured tracking that Level 0 organisations lack. At Levels 2+, automated validation can test Data and Ops assumptions continuously; the register tracks which assumptions have been automated and which still require manual validation.

### Google Rules of Machine Learning

* **Rule 4: "Keep the first model simple and get the infrastructure right."** The register captures the assumptions underlying the first model, ensuring they are tested rather than inherited from planning documents.
* **Rule 10: "Watch for silent failures."** Assumptions that degrade silently (data representativeness, label quality) are tracked in the register and surfaced through the burn-down.

### Summary of Alignment

| Framework | Primary Contribution | Gap the Assumption Register Fills |
| --- | --- | --- |
| Microsoft MLOps Maturity Model | Experiment tracking and automation | Explicit assumption tracking with status, evidence, and burn-down metrics |
| Google Rules of ML | Practitioner heuristics for simplicity and silent failure detection | Structured mechanism for recording, testing, and retiring programme assumptions |

## References

* Microsoft. "MLOps maturity model." Azure Architecture Center. <https://learn.microsoft.com/azure/architecture/ai-ml/guide/mlops-maturity-model>
* Zinkevich, Martin. "Rules of Machine Learning: Best Practices for ML Engineering." Google Developers. <https://developers.google.com/machine-learning/guides/rules-of-ml>
* Google. "MLOps: Continuous delivery and automation pipelines in machine learning." Google Cloud Architecture Center. <https://cloud.google.com/architecture/mlops-continuous-delivery-and-automation-pipelines-in-machine-learning>
* AWS. "Machine Learning Lens -- AWS Well-Architected Framework." Amazon Web Services. <https://docs.aws.amazon.com/wellarchitected/latest/machine-learning-lens/machine-learning-lens.html>
