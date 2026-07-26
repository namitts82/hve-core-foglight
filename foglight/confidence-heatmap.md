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

**Confidence Heatmap for Experiments**

ADO Work Item: [Confidence Heatmap for experiments](https://dev.azure.com/PMaaS2022/Foglight/_workitems/edit/921)
Parent Epic: [Identifying Gaps in Existing TPM Tools, Language, and Signals](https://dev.azure.com/PMaaS2022/Foglight/_workitems/edit/903)

## Problem and Purpose

Experiments in AI/ML programs generate results. Results alone do not tell the TPM whether a program is converging toward a decision or spinning in place. Traditional experiment reporting focuses on outcomes: accuracy improved, latency decreased, the model outperformed the baseline. This framing creates a set of failure modes:

* Teams report experiment results as "good" or "bad" based on a single metric, collapsing multiple distinct dimensions of evidence into a binary judgment that hides where uncertainty actually lives
* Experiments that produce poor performance metrics are labelled as failures, even when they meaningfully reduced uncertainty about the problem space, invalidated a flawed assumption, or narrowed the viable approach set
* Stakeholders cannot distinguish between an experiment that produced strong results on clean data and one that produced equivalent results on representative production data, because no structured record separates data quality from model performance
* Teams run sequences of experiments without tracking how each experiment shifted confidence across dimensions, making it impossible to detect when overall confidence has stalled despite continued activity
* Phase-exit and pivot decisions rely on the most recent experiment result rather than the accumulated evidence pattern across a series of experiments
* No mechanism connects an experiment outcome to the specific decision the experiment was designed to inform, so experiments become detached from the program's decision roadmap

The Confidence Heatmap for Experiments solves these problems by decomposing each experiment's contribution into six dimensions of confidence, assessing each dimension independently, and tracking movement across experiment sequences. An experiment is successful if it changes the heatmap. The heatmap gives the TPM a structured instrument for evaluating what each experiment changed in confidence, connecting experiment outcomes to decisions, and detecting when a series of experiments has stopped generating useful evidence.

## Project Phases

The Confidence Heatmap is used whenever experiments occur. Its primary operating window spans Exploration and Iteration and Evaluation, where experiments are the dominant mode of work. It can also appear in Scoping and Framing (when feasibility spikes produce experiment-like outputs) and Delivery and Integration (when integration validation generates new evidence).

::: mermaid
graph LR
  S[Scoping and Framing]
  E[Exploration]
  I[Iteration and Evaluation]
  D[Delivery and Integration]
  O[Operations]

  S --> E --> I --> D --> O

  S -.- U1[Assess feasibility spikes<br>using heatmap dimensions]
  E -.- U2[Heatmap every experiment.<br>Track movement across series.]
  I -.- U3[Use aggregated trends to<br>support pivot/commit decisions]
  D -.- U4[Validate integration experiments<br>against heatmap dimensions]
  O -.- U5[Post-deployment experiments<br>feed back into heatmap]

  style S fill:#FEF3C7,stroke:#F59E0B
  style E fill:#DBEAFE,stroke:#3B82F6
  style I fill:#DCFCE7,stroke:#22C55E
  style D fill:#FCE7F3,stroke:#EC4899
  style O fill:#E0E7FF,stroke:#6366F1
:::

| Phase | Confidence Heatmap Role |
| --- | --- |
| Scoping and Framing | Apply the heatmap to feasibility spikes or early proof-of-concept investigations. The heatmap captures what was learned about the problem, data, and approach before formal experimentation begins. |
| Exploration | Primary operating phase. Every experiment produces a heatmap. The TPM tracks movement across successive experiments to detect convergence, stalls, and emerging risks. Aggregated views across three to five experiments inform mid-phase direction changes. |
| Iteration and Evaluation | Aggregated trend analysis across the full experiment series supports the pivot, commit, or stop decision at phase exit. The heatmap record provides the evidence trail for the decision. |
| Delivery and Integration | Integration validation experiments (end-to-end testing, production-data evaluation, workflow integration checks) produce heatmaps that assess whether lab-validated confidence holds in the real environment. |
| Operations | Post-deployment experiments (retraining evaluations, drift investigations, performance investigations on new data slices) produce heatmaps that feed back into the Confidence Dashboard's operational dimensions. |

## Tool Description

The Confidence Heatmap operates at the experiment level, not the programme level. Where the [Confidence Dashboard](https://dev.azure.com/PMaaS2022/Foglight/_workitems/edit/920) tracks six programme-wide dimensions across the engagement lifecycle, the Confidence Heatmap assesses what a single experiment contributed to the programme's understanding. The two tools are complementary: experiment-level heatmaps feed evidence into programme-level dashboard updates.

### Experiment Confidence Dimensions

The heatmap decomposes each experiment's contribution into six dimensions. These dimensions are experiment-specific assessments, not lifecycle-wide status indicators.

::: mermaid
graph TD
  EH[Experiment<br>Confidence Heatmap]

  HC[Hypothesis Confidence<br>Did this experiment<br>meaningfully test the<br>hypothesis?]
  SS[Signal Strength<br>Are results consistent<br>and reproducible?]
  SN[Sensitivity / Stability<br>How fragile are results<br>to small changes?]
  DR[Data Representativeness<br>How realistic was the<br>data used?]
  MV[Metric Validity<br>Do the metrics reflect<br>real value?]
  DRE[Decision Readiness<br>Does this enable a<br>concrete decision?]

  EH --> HC
  EH --> SS
  EH --> SN
  EH --> DR
  EH --> MV
  EH --> DRE

  style EH fill:#FDE68A,stroke:#F59E0B,stroke-width:2px
  style HC fill:#DBEAFE,stroke:#3B82F6
  style SS fill:#DCFCE7,stroke:#22C55E
  style SN fill:#FECACA,stroke:#EF4444
  style DR fill:#E0E7FF,stroke:#6366F1
  style MV fill:#FCE7F3,stroke:#EC4899
  style DRE fill:#FEF3C7,stroke:#F59E0B
:::

| Dimension | What It Measures | Example Evidence |
| --- | --- | --- |
| Hypothesis Confidence | Whether the experiment meaningfully tested the stated hypothesis. A narrow test that cleanly isolates one variable scores higher than a broad test that conflates multiple factors. | Clear experimental control, single variable isolation, hypothesis directly addressed by the outcome |
| Signal Strength | Whether results are consistent and reproducible across runs, segments, and conditions. | Metric variance across repeated runs, consistency across data slices, inter-annotator agreement where applicable |
| Sensitivity / Stability | How fragile the results are to small changes in inputs, parameters, or conditions. Low stability indicates the approach may not generalise. | Performance change when features are perturbed, sensitivity to hyperparameter shifts, robustness under noise injection |
| Data Representativeness | How closely the experiment data matched real production conditions. Synthetic, downsampled, or pre-cleaned data scores lower than representative production data. | Data source description, coverage of production segments, presence of edge cases, known distribution gaps |
| Metric Validity | Whether the metrics used in the experiment reflect the actual business or user value the programme targets. Proxy metrics score lower than direct measures. | Alignment between experiment metric and agreed success criteria, stakeholder validation of metric relevance |
| Decision Readiness | Whether the experiment outcome enables a concrete programme decision (continue, pivot, narrow scope, stop). An experiment that produces interesting results but does not inform a decision scores lower. | Explicit mapping to a pending decision, clarity of the next action, sufficiency of evidence for the decision |

### Confidence Scale

Each dimension uses a three-level scale. The scale is intentionally coarse to prevent false precision and force clear judgment.

::: mermaid
graph LR
  LOW[🔴 Low<br>High uncertainty,<br>weak or misleading signal]
  MED[🟡 Medium<br>Partial evidence,<br>directional insight]
  HIGH[🟢 High<br>Strong evidence,<br>decision-enabling]

  LOW --> MED --> HIGH

  style LOW fill:#FCA5A5,stroke:#DC2626
  style MED fill:#FDE68A,stroke:#F59E0B
  style HIGH fill:#DCFCE7,stroke:#22C55E
:::

| Level | Meaning | TPM Action |
| --- | --- | --- |
| 🔴 Low | High uncertainty. Weak or misleading signal. The experiment did not produce usable evidence on this dimension. | Investigate whether the experiment design was flawed for this dimension, or whether the dimension needs targeted follow-up work. |
| 🟡 Medium | Partial evidence. Directional insight exists but is not sufficient to support a confident decision. | Note the caveat and plan targeted follow-up. Track whether successive experiments move this dimension. |
| 🟢 High | Strong evidence. The experiment produced clear, decision-enabling results on this dimension. | Record the evidence and connect it to the decision the experiment informs. No further work needed on this dimension unless conditions change. |

No numbers. No averages. The heatmap is a qualitative judgment instrument, not a scoring system.

### Reading the Heatmap

The heatmap's value comes from pattern recognition, not from counting colours.

**A good experiment outcome:**

* Some 🔴 remain -- uncertainty persists in dimensions the experiment was not designed to address
* At least one 🟡 to 🟢 shift -- meaningful evidence was produced
* A clear next action is enabled -- the experiment connected to the decision roadmap

**A bad experiment outcome:**

* Everything 🟢 with no caveats -- this usually indicates false confidence or an insufficiently rigorous assessment
* Everything 🔴 with no learning extracted -- the experiment was either poorly designed or poorly interpreted
* No clear decision implication -- the experiment produced results but did not advance the programme's decision roadmap

**The Foglight rule:** An experiment is successful if it changes the heatmap. If nothing moves, the experiment was poorly designed or poorly interpreted.

### Kill / Pivot / Continue Signals

Each heatmap includes a directional signal at the bottom:

| Signal | Meaning |
| --- | --- |
| 🚨 Continue | Confidence is increasing in key dimensions. The current approach is producing evidence. |
| 🟡 Pivot | Signal is present but fragility is too high, or the approach is producing diminishing returns. Consider changing the experimental strategy. |
| ⛔ Stop | Confidence is decreasing or the approach is misaligned with the target impact. Escalate for a programme-level decision. |

These signals give the TPM a defensible decision narrative for each experiment outcome.

### Relationship to the Confidence Dashboard

::: mermaid
graph LR
  E1[Experiment 1<br>Heatmap] --> AGG[Aggregated<br>Trend View]
  E2[Experiment 2<br>Heatmap] --> AGG
  E3[Experiment 3<br>Heatmap] --> AGG
  AGG --> CD[Confidence Dashboard<br>Programme-Level Update]

  style E1 fill:#DBEAFE,stroke:#3B82F6
  style E2 fill:#DBEAFE,stroke:#3B82F6
  style E3 fill:#DBEAFE,stroke:#3B82F6
  style AGG fill:#FDE68A,stroke:#F59E0B
  style CD fill:#DCFCE7,stroke:#22C55E
:::

Individual experiment heatmaps feed into an aggregated trend view across three to five experiments. The aggregated view then provides evidence for updating the programme-level [Confidence Dashboard](https://dev.azure.com/PMaaS2022/Foglight/_workitems/edit/920). This connection ensures that experiment-level learning is captured, preserved, and propagated into stakeholder-facing progress reports.

### Responsibility Alignment

The Confidence Heatmap directly supports three Foglight responsibility areas:

| Responsibility | How the Confidence Heatmap Supports It |
| --- | --- |
| [Progress Tracking](https://dev.azure.com/PMaaS2022/Foglight/_workitems/edit/960) | The heatmap makes experimental progress visible through confidence movement rather than activity counts. It tracks uncertainty reduction per experiment, surfaces stalled dimensions across experiment sequences, and provides the evidence basis for progress claims. When no dimensions move across consecutive experiments, the heatmap signals that activity is not producing progress. |
| [Definition of Done](https://dev.azure.com/PMaaS2022/Foglight/_workitems/edit/962) | The heatmap redefines what a "done" experiment means. An experiment is done when it has been assessed across all six dimensions, its contribution to the decision roadmap is recorded, and the next action is identified. Decision Readiness directly measures whether the experiment moved the programme closer to a gate decision. |
| [Risk Management](https://dev.azure.com/PMaaS2022/Foglight/_workitems/edit/955) | The heatmap surfaces residual risk through Sensitivity/Stability (fragile results that may not generalise), Data Representativeness (lab conditions that do not reflect production), and Signal Strength (inconsistent results that may reverse). The kill/pivot/continue signal provides an early warning mechanism for approaches that are accumulating risk rather than reducing it. |

## How to Use

### Step 1: Record Experiment Metadata

Before running the experiment, capture the experiment name, the hypothesis being tested, the baseline or comparator, and the specific decision the experiment is designed to inform. This metadata anchors the heatmap to the programme's decision roadmap and prevents experiments from drifting into untethered exploration.

### Step 2: Run the Experiment and Collect Results

Execute the experiment according to the agreed protocol. Collect performance metrics, variance data, slice-level results, and any qualitative observations about data quality, failure modes, or unexpected behaviours.

### Step 3: Assess Each Dimension

After the experiment completes, assess each of the six dimensions independently. For each dimension, record the level (🔴 Low, 🟡 Medium, 🟢 High), the evidence observed, key caveats, and what this dimension enables next. Resist the temptation to average or aggregate across dimensions. The heatmap's value comes from dimension-level specificity.

### Step 4: Assign the Kill / Pivot / Continue Signal

Based on the dimension assessment, assign the overall directional signal. Use the patterns described in "Reading the Heatmap" to guide this judgment. If the signal is ⛔ Stop or 🟡 Pivot, document the specific dimensions driving that assessment.

### Step 5: Update the Aggregated View

After every three to five experiments, update the aggregated trend view. For each dimension, record the direction of movement (↑ improving, → flat, ↓ declining) and interpret the pattern. The aggregated view supports the phase-exit decision and feeds into the programme-level Confidence Dashboard update.

::: mermaid
flowchart TD
  START[Experiment Completed] --> META[Record experiment<br>metadata and results]
  META --> ASSESS[Assess each of six<br>dimensions independently]
  ASSESS --> SIGNAL{Assign Kill / Pivot /<br>Continue signal}
  SIGNAL -- Continue --> COUNT{Three to five<br>experiments since<br>last aggregation?}
  SIGNAL -- Pivot --> REVIEW[Document pivot rationale.<br>Redesign next experiment.]
  SIGNAL -- Stop --> ESCALATE[Escalate to programme<br>decision review]
  COUNT -- No --> NEXT[Design next experiment<br>targeting weakest dimensions]
  COUNT -- Yes --> AGGREGATE[Update aggregated<br>trend view]
  AGGREGATE --> DASHBOARD[Feed evidence into<br>Confidence Dashboard update]
  DASHBOARD --> GATE{Phase gate<br>approaching?}
  GATE -- Yes --> DECIDE[Present aggregated evidence<br>for pivot / commit / stop]
  GATE -- No --> NEXT
  REVIEW --> NEXT
  ESCALATE --> DECIDE

  style START fill:#DBEAFE,stroke:#3B82F6
  style ESCALATE fill:#FCA5A5,stroke:#DC2626
  style DECIDE fill:#FCE7F3,stroke:#EC4899
  style DASHBOARD fill:#DCFCE7,stroke:#22C55E
  style AGGREGATE fill:#FDE68A,stroke:#F59E0B
:::

## Reusable Components

### Experiment Heatmap Template

The core assessment artifact. One row per dimension, completed after each experiment.

| Confidence Dimension | Level | Evidence Observed | Key Caveats | What This Enables Next |
| --- | --- | --- | --- | --- |
| Hypothesis Confidence | 🔴 / 🟡 / 🟢 | *What the experiment showed about the hypothesis* | *Limitations of the test design* | *Follow-up needed or decision unlocked* |
| Signal Strength | 🔴 / 🟡 / 🟢 | | | |
| Sensitivity / Stability | 🔴 / 🟡 / 🟢 | | | |
| Data Representativeness | 🔴 / 🟡 / 🟢 | | | |
| Metric Validity | 🔴 / 🟡 / 🟢 | | | |
| Decision Readiness | 🔴 / 🟡 / 🟢 | | | |

**Signal:** 🚨 Continue / 🟡 Pivot / ⛔ Stop

### Experiment Metadata Header

Attach this header to every heatmap to maintain traceability.

| Field | Value |
| --- | --- |
| Experiment Name | *Descriptive name* |
| Hypothesis Tested | *The specific hypothesis this experiment addresses* |
| Baseline / Comparator | *What this experiment is compared against* |
| Decision This Informs | *The programme decision this experiment advances* |
| Date | *Experiment completion date* |
| Sprint / Cycle | *Learning cycle identifier* |

### Aggregated Trend View

After three to five experiments, build the trend view. Use direction indicators, not averages.

| Dimension | Trend | Interpretation |
| --- | --- | --- |
| Hypothesis Confidence | ↑ / → / ↓ | *What the trend means for the approach direction* |
| Signal Strength | ↑ / → / ↓ | *Whether results are becoming more or less reproducible* |
| Sensitivity / Stability | ↑ / → / ↓ | *Whether the approach is becoming more or less robust* |
| Data Representativeness | ↑ / → / ↓ | *Whether experiments are moving closer to production conditions* |
| Metric Validity | ↑ / → / ↓ | *Whether metrics are becoming better aligned with real value* |
| Decision Readiness | ↑ / → / ↓ | *Whether the experiment series is converging toward a decision* |

**Overall assessment:** Continue / Pivot / Stop with rationale.

### TPM Language Reference Card

Use this card to reframe experiment outcomes in confidence-movement language.

| Instead of | Say |
| --- | --- |
| "Experiment failed." | "This experiment reduced confidence in stability but increased confidence that feature X matters." |
| "Accuracy went down." | "This invalidated our assumption about data cleanliness, which is valuable." |
| "Results are inconclusive." | "Signal Strength remains at Low. We need to redesign the experiment to isolate the variable." |
| "We need more experiments." | "Decision Readiness is at Medium. One targeted experiment on [specific dimension] would move it to High." |

## Sample Use Case

### Scenario: Predictive Maintenance for a Manufacturing Fleet

A manufacturer operates 1,200 CNC milling machines across four plants. The engineering team wants to predict spindle bearing failures 48 to 72 hours before breakdown, allowing maintenance crews to schedule replacements during planned downtime. The engagement spans 14 weeks: 3 weeks of scoping, 6 weeks of exploration (three planned experiments), 3 weeks of evaluation, and 2 weeks of production-readiness preparation. The plant operations director holds the proceed/stop authority.

This is a probabilistic system because:

* Vibration signatures vary across machine age, workload profile, and bearing manufacturer
* Sensor data arrives at varying frequencies with noise from ambient factory conditions
* The model must outperform the existing time-based replacement schedule (every 2,000 operating hours) to justify the capital investment in monitoring infrastructure
* False positive alerts waste maintenance crew time; false negatives cause unplanned downtime at an average cost of $42,000 per incident

### Experiment 1: Baseline Anomaly Detection on Vibration Data (Week 4)

**Hypothesis:** A statistical anomaly detection model (Isolation Forest) applied to raw vibration frequency data can distinguish healthy bearings from bearings approaching failure, using historical maintenance records as labels.

| Confidence Dimension | Level | Evidence Observed | Key Caveats | What This Enables Next |
| --- | --- | --- | --- | --- |
| Hypothesis Confidence | 🟢 High | Isolation Forest correctly flagged 78% of historically recorded failures in the 90-day lookback dataset. Hypothesis directly tested with clear positive/negative splits. | Labelling relied on maintenance logs, which may miss slow-degradation failures that were caught during scheduled replacement. | Validates that vibration data contains a detectable failure signal. Proceed to feature engineering. |
| Signal Strength | 🟡 Medium | Precision 0.71, recall 0.78. Results consistent across three random seeds. | Performance varies across plants: Plant A recall 0.84, Plant D recall 0.62. Plant D machines are 8 years older on average. | Investigate plant-age interaction. Feature engineering should account for machine age. |
| Sensitivity / Stability | 🔴 Low | Removing two of eight vibration frequency bands caused recall to drop by 19 points. Model is highly sensitive to input feature composition. | Feature importance is concentrated in two bands. Loss of either sensor channel would degrade performance severely. | Feature engineering must build redundancy. Evaluate whether additional sensor channels (temperature, current draw) reduce fragility. |
| Data Representativeness | 🟡 Medium | 14 months of historical data covering 940 of 1,200 machines. 260 machines in Plant D lacked continuous vibration logging until 6 months ago. | Plant D is underrepresented. No edge cases for unusual workload profiles (night-shift surge runs). | Extend Plant D data collection. Request night-shift workload samples for Experiment 2. |
| Metric Validity | 🟡 Medium | Precision and recall are standard and interpretable. However, the business case depends on lead time (48-72 hours before failure), which this experiment did not measure. | Lead time not yet assessed. A model with high recall but short lead time would not meet the operational requirement. | Experiment 2 must measure prediction lead time as a primary metric alongside precision/recall. |
| Decision Readiness | 🟡 Medium | Confirms that a vibration-based approach is viable. Does not yet inform the commit/pivot decision because stability and lead time are unresolved. | Two dimensions at Low or early Medium block the commit decision. | Proceed to Experiment 2 with expanded features and lead-time measurement. |

**Signal:** 🚨 Continue -- Confidence increased in key dimensions. Fragility and lead-time measurement are the priority for the next experiment.

### Experiment 2: Feature Engineering with Multi-Sensor Fusion (Week 7)

**Hypothesis:** Combining vibration frequency data with temperature readings and motor current draw, and adding machine-age features, will improve stability and extend prediction lead time beyond the 48-hour threshold.

| Confidence Dimension | Level | Evidence Observed | Key Caveats | What This Enables Next |
| --- | --- | --- | --- | --- |
| Hypothesis Confidence | 🟢 High | Multi-sensor feature set directly tested against vibration-only baseline. Feature ablation study confirmed that each additional sensor channel contributes independent information. | Temperature sensors in Plant B have a 4-hour logging gap during shift changes. Gap-filling with interpolation may introduce artefacts. | Hypothesis confirmed. Multi-sensor fusion is the correct direction. Address Plant B logging gap operationally. |
| Signal Strength | 🟢 High | Precision 0.79, recall 0.83. Five-fold cross-validation AUROC range: 0.81 to 0.86. Plant-level variance reduced: Plant D recall improved from 0.62 to 0.76 with machine-age features. | Plant D still 7 points below fleet average. | Acceptable variance for proceed decision. Plant D gap is within operational tolerance if monitoring is plant-aware. |
| Sensitivity / Stability | 🟡 Medium | Removing any single sensor channel causes at most 8-point recall drop (compared to 19 points in Experiment 1). Model degrades gracefully under single-channel failure. | Simultaneous loss of two channels (vibration + temperature) causes 22-point drop. Multi-channel redundancy is necessary but not infinite. | Operational deployment must include sensor health monitoring. Dual-channel failure triggers fallback to time-based schedule. |
| Data Representativeness | 🟡 Medium | Night-shift workload samples added for 3 plants. Coverage now includes 1,080 of 1,200 machines across all shift patterns. | 120 machines in Plant D still lack full sensor coverage. Night-shift data covers only 6 weeks. | Sufficient for commit decision if the 120-machine gap is documented as a known limitation. Extended monitoring will close the gap post-deployment. |
| Metric Validity | 🟢 High | Median prediction lead time: 56 hours. 82% of true positive predictions occurred within the 48-72 hour target window. Lead time now measured as a primary metric alongside precision/recall. | 18% of predictions fall outside the target window (12% too early, 6% too late). "Too late" predictions carry higher operational risk. | Lead-time metric validates the operational use case. "Too late" rate of 6% must be compared against the current unplanned failure rate (11% of replacements are reactive). |
| Decision Readiness | 🟡 Medium | Direction is clear. Multi-sensor fusion with machine-age features is the production approach. Remaining question: does the model transfer to machines it has never seen (new installations)? | Transfer learning for new machines not yet tested. 40 machines installed in the last 3 months have no failure history. | Experiment 3 must test cold-start prediction on new machines using fleet-average priors. |

**Signal:** 🚨 Continue -- Four dimensions at High or solid Medium. Stability improved. Decision Readiness depends on the cold-start transfer question.

### Experiment 3: Transfer Learning for New Machine Cold-Start (Week 9)

**Hypothesis:** A fleet-average prior model, fine-tuned with 2 weeks of operational data from a new machine, can achieve recall within 10 points of the full-history model within the first month of operation.

| Confidence Dimension | Level | Evidence Observed | Key Caveats | What This Enables Next |
| --- | --- | --- | --- | --- |
| Hypothesis Confidence | 🟢 High | Fleet-average prior tested on 40 new machines using leave-one-out simulation. After 2 weeks of data, fine-tuned model achieved recall 0.74 versus 0.83 for full-history model. 9-point gap is within the 10-point threshold. | Simulation used retrospective data from machines that later accumulated full history. True cold-start performance on genuinely new machines will be validated during pilot. | Hypothesis confirmed within tolerance. Cold-start approach is viable for production deployment. |
| Signal Strength | 🟢 High | Fine-tuned model performance converges to within 5 points of the full-history model after 4 weeks of data. Convergence is monotonic and consistent across machine types. | Convergence rate may differ for machine types not represented in the current fleet (future procurement). | Acceptable for commit decision. Future machine types require a monitoring protocol to verify convergence. |
| Sensitivity / Stability | 🟡 Medium | Fleet-average prior is robust to single-channel sensor failure (7-point recall drop). Fine-tuning with limited data is moderately sensitive to data quality: a corrupted sensor day caused a 12-point transient dip that recovered after 3 days. | Early-life data quality matters more for cold-start machines. Sensor validation during the first 2 weeks is critical. | Operational protocol must include sensor health checks during the cold-start window. Automated data quality flags should suppress fine-tuning on corrupted data. |
| Data Representativeness | 🟡 Medium | 40 new machines across 3 plants. Covers 3 of 4 machine types in the fleet. Does not include the newest machine type (installed in Plant C, 6 units). | Newest machine type untested. Small sample of 40 may not capture all installation variation. | Document as a known limitation. Monitor newest machine type performance during pilot. |
| Metric Validity | 🟢 High | Same lead-time metric used across all three experiments. Cold-start median lead time: 52 hours (within target). | Slightly lower lead time than the full-history model (56 hours). Operationally acceptable. | Metrics are consistent and aligned with the business case. |
| Decision Readiness | 🟢 High | All three experiments together provide sufficient evidence for the commit decision. Approach is validated. Cold-start gap is within tolerance. Known limitations are documented. | Remaining items are operational: sensor monitoring, Plant D coverage, newest machine type observation. These are deployment conditions, not experiment questions. | Present aggregated evidence to the plant operations director for the proceed/conditional-proceed decision. |

**Signal:** 🚨 Continue to commit decision.

### Aggregated Trend View (Experiments 1 through 3)

| Dimension | Exp 1 | Exp 2 | Exp 3 | Trend | Interpretation |
| --- | --- | --- | --- | --- | --- |
| Hypothesis Confidence | 🟢 | 🟢 | 🟢 | → (stable High) | Approach direction confirmed early and sustained. No need for further hypothesis-level testing. |
| Signal Strength | 🟡 | 🟢 | 🟢 | ↑ | Reproducibility improved as feature set expanded. Plant-level variance reduced to acceptable levels. |
| Sensitivity / Stability | 🔴 | 🟡 | 🟡 | ↑ | Fragility significantly reduced by multi-sensor fusion. Remaining sensitivity is operational (sensor health) rather than algorithmic. |
| Data Representativeness | 🟡 | 🟡 | 🟡 | → (stable Medium) | Coverage expanded incrementally but did not reach High. Known gaps (Plant D subset, newest machine type) are documented as deployment conditions. |
| Metric Validity | 🟡 | 🟢 | 🟢 | ↑ | Lead-time metric added in Experiment 2 and validated in Experiment 3. Metrics now fully aligned with the business case. |
| Decision Readiness | 🟡 | 🟡 | 🟢 | ↑ | Converging toward commit. Each experiment closed a specific gap (viability, stability, cold-start). Remaining open items are operational, not experimental. |

**Overall assessment:** Proceed to commit decision with documented conditions for Plant D coverage, newest machine type monitoring, and sensor health protocols.

### What the Confidence Heatmap Revealed

Without the Confidence Heatmap, this programme would have faced three risks common in manufacturing AI deployments:

* **Experiment 1 would have been declared a success prematurely.** The 78% recall headline looked promising. The heatmap forced the team to confront the 🔴 Low Sensitivity/Stability rating, revealing that the model was dangerously dependent on two vibration frequency bands. Without that signal, the team might have moved directly to deployment and discovered the fragility in production when a sensor channel degraded.
* **The lead-time gap would have surfaced at the production gate.** Experiment 1 never measured prediction lead time. Traditional experiment reporting would have carried precision and recall forward as proof of viability. The heatmap's Metric Validity dimension at 🟡 Medium flagged that the experiment had not yet validated the metric that mattered most to the plant operations director. This forced lead-time measurement into Experiment 2 rather than discovering the gap at the gate review.
* **The cold-start question would have been deferred into production.** Without the heatmap's Decision Readiness tracking, the team might have presented Experiment 2 results as sufficient evidence for the commit decision. The 🟡 Medium Decision Readiness rating in Experiment 2 made the transfer-learning question explicit, leading to Experiment 3. The aggregated trend view showed the decision committee exactly how each experiment narrowed the remaining uncertainty.

## Industry Framework Alignment

The Confidence Heatmap draws on concepts that appear across established frameworks for managing ML systems, experiment design, and production readiness. This section maps the tool's design to four frameworks to show where Foglight's approach aligns, extends, or fills gaps.

### Microsoft MLOps Maturity Model

Microsoft's MLOps maturity model defines five levels (0 through 4) describing an organisation's capability to develop, deploy, and operate ML systems. At Level 0, all processes are manual and experiments are not tracked consistently. At Level 4, deployed models emit centralized metrics and drift or regression signals trigger automatic retraining.

The Confidence Heatmap maps to the MLOps maturity model across its experiment management dimensions:

* **Experiment tracking maturity.** At MLOps Level 0, "experiments aren't tracked consistently" and the end result is a single model file handed off manually. The Confidence Heatmap directly addresses this gap by requiring structured assessment of every experiment across six dimensions. Even at Level 0 maturity, a team using the heatmap produces a traceable record of what each experiment contributed.
* **Model validation and reproducibility.** At Level 2, model training is automated and performance tracking is centralized. The heatmap's Signal Strength dimension tracks the same concern at the individual experiment level: are results consistent and reproducible? At Level 3, A/B testing of model performance is integrated for deployment, which aligns with the heatmap's Decision Readiness dimension -- assessing whether evidence is sufficient for a deployment decision.
* **Data validation alignment.** At Level 4, feature materialization health and freshness are monitored. The heatmap's Data Representativeness dimension tracks a precursor to this capability: whether the data used in each experiment is representative enough to trust the results. Teams that assess Data Representativeness per experiment build the judgment needed to later design automated data validation pipelines.

The gap the Confidence Heatmap fills: the MLOps maturity model describes organisational capabilities and pipeline automation. It does not prescribe how to assess what an individual experiment contributed to programme understanding. A team at MLOps Level 2 with automated training may still lack a structured way to evaluate whether a specific experiment moved the programme closer to a decision. The heatmap provides this experiment-level assessment instrument, translating MLOps concerns into per-experiment confidence dimensions that a TPM can evaluate and communicate.

### Google MLOps and Rules of Machine Learning

Google's MLOps framework describes three automation levels (0, 1, and 2) for ML pipelines. Martin Zinkevich's Rules of Machine Learning provides 43 practical rules from Google's production ML experience. Together, these resources define both the operational infrastructure and the practitioner judgment needed to manage ML experiments through their lifecycle.

The Confidence Heatmap operationalises several of these concepts:

* **Rule 2: "First, design and implement metrics."** This rule establishes that measurement infrastructure must precede model development. The heatmap's Metric Validity dimension enforces this principle at the experiment level: each experiment must assess whether its metrics actually reflect real value. Proxy metrics are explicitly flagged as lower confidence than direct measures.
* **Rules 8 through 11: Silent failures, freshness, and staleness.** Rule 8 (know the freshness requirements) and Rule 10 (watch for silent failures) describe risks that erode experiment reliability without visible warning. The heatmap's Signal Strength dimension captures reproducibility concerns that may indicate a silent failure. Data Representativeness captures freshness and coverage concerns that Rule 8 highlights.
* **Rule 38: "Don't waste time on new features if unaligned objectives have become the issue."** This rule identifies the failure mode where teams continue technical work when the real problem is strategic misalignment. The heatmap's Decision Readiness dimension captures this: if experimental results improve but Decision Readiness stays at 🔴 Low, the problem may be that the experiment is answering the wrong question, not that it needs better features.
* **MLOps Level 1: Data and model validation.** At Level 1, automated data validation detects schema skews and data value skews. The heatmap's Data Representativeness and Signal Strength dimensions track these concerns before automation exists. They build the assessment habit that later scales into automated validation.
* **Training-serving skew (Rules 29-37).** These rules address the gap between training conditions and production conditions. The heatmap's Sensitivity/Stability dimension tracks a related concern at the experiment level: how fragile are results to changes in input conditions? High sensitivity in an experiment predicts training-serving skew problems in production.

The gap the Confidence Heatmap fills: Google's rules are practitioner-oriented guidance for ML engineers. They describe what to watch for but do not prescribe how to aggregate experiment-level findings into programme decisions, or how to communicate experiment quality to non-technical stakeholders. The heatmap translates practitioner observations (silent failures, metric validity, reproducibility) into a structured six-dimension assessment that stakeholders can read and that feeds directly into phase-gate decisions.

### AWS Well-Architected Framework: Machine Learning Lens

The AWS Well-Architected Framework's Machine Learning Lens applies six architectural pillars to ML workloads: Operational Excellence, Reliability, Performance Efficiency, Cost Optimisation, Security, and Sustainability. While the Lens focuses on production architecture, its principles have direct implications for experiment quality assessment.

The Confidence Heatmap maps the Lens pillars to experiment-level concerns:

* **Operational Excellence maps to Decision Readiness.** The pillar emphasises automation, monitoring, and continuous improvement. At the experiment level, Decision Readiness tracks whether the experiment is producing evidence that can be operationalised -- whether the outcome informs a concrete decision rather than generating interesting but unactionable results.
* **Reliability maps to Signal Strength and Sensitivity/Stability.** The pillar focuses on system resilience and consistent behaviour. At the experiment level, Signal Strength tracks whether results are reproducible, and Sensitivity/Stability tracks whether results are robust to perturbation. Experiments that score poorly on these dimensions predict reliability problems in production.
* **Performance Efficiency maps to Data Representativeness.** The pillar addresses efficient use of resources to meet requirements. At the experiment level, Data Representativeness tracks whether the experiment conditions match the performance demands of the real environment. An experiment that performs well on clean, pre-processed data may fail under production data loads.
* **Security maps to Data Representativeness.** The pillar addresses data protection and compliance. At the experiment level, Data Representativeness encompasses whether data governance and privacy constraints were reflected in the experiment design -- whether the data used is not only technically representative but also permissible.

The gap the Confidence Heatmap fills: the ML Lens prescribes architectural best practices for production systems. It does not address how to evaluate individual experiments during the exploration phase. The heatmap translates architectural concerns into experiment-level assessment dimensions, creating a bridge between exploration-phase evidence and the production-quality standards the Lens defines. Teams that assess experiments against heatmap dimensions during exploration are more likely to meet Well-Architected standards at deployment.

### Design of Experiments and Statistical Experiment Design

The statistical Design of Experiments (DOE) tradition, rooted in the work of Ronald Fisher and extended through factorial design, response surface methodology, and modern A/B testing frameworks, provides rigorous methods for structuring experiments to isolate causal effects, control for confounding variables, and assess statistical significance.

The Confidence Heatmap draws on DOE principles while adapting them for the programme management context:

* **Variable isolation maps to Hypothesis Confidence.** DOE's core principle is that an experiment should test one variable at a time (or use factorial designs that can separate main effects from interactions). The heatmap's Hypothesis Confidence dimension assesses whether the experiment meaningfully isolated the hypothesis from confounding factors. A broadly scoped experiment that changes multiple variables simultaneously scores lower on this dimension.
* **Replication and reproducibility map to Signal Strength.** DOE requires replicated trials to distinguish signal from noise. Signal Strength tracks the same concern: are results consistent across runs, random seeds, and data splits? Low Signal Strength indicates that variance is too high to trust the result, mirroring DOE's requirement for statistical power.
* **Robustness testing maps to Sensitivity/Stability.** Taguchi's robust design methodology extends classical DOE by testing how sensitive outputs are to noise factors. The heatmap's Sensitivity/Stability dimension captures this: how fragile are results to small perturbations in inputs, parameters, or conditions? This dimension directly reflects the DOE principle that a robust result should hold across a range of operating conditions.
* **External validity maps to Data Representativeness.** In DOE, external validity asks whether experimental findings generalise beyond the specific conditions of the experiment. Data Representativeness tracks the same question: does the data used in the experiment reflect the real production environment closely enough to trust the results in deployment?

The gap the Confidence Heatmap fills: classical DOE provides statistical rigour for experiment design and analysis. It does not address how to connect experiment outcomes to programme-level decisions, how to track confidence movement across a sequence of experiments, or how to communicate experiment quality to non-technical stakeholders. The heatmap bridges the gap between statistical experiment quality and programme delivery by adding Decision Readiness and Metric Validity dimensions that have no direct equivalent in classical DOE, and by providing an aggregated trend view that makes experiment sequences interpretable at the programme level.

### Summary of Alignment

| Framework | Primary Contribution | Gap the Confidence Heatmap Fills |
| --- | --- | --- |
| Microsoft MLOps Maturity Model | Organisational capability levels for experiment tracking, model validation, and automated operations | Per-experiment assessment instrument that tracks what each experiment contributed, regardless of organisation-level automation maturity |
| Google MLOps and Rules of ML | Practitioner heuristics for metric design, silent failure detection, reproducibility, and training-serving skew | Translation of engineering heuristics into a structured six-dimension framework that connects individual experiments to programme decisions |
| AWS Well-Architected ML Lens | Architectural best practices across six pillars for production ML systems | Experiment-level assessment that bridges exploration-phase evidence to production-quality standards |
| Design of Experiments (DOE) | Statistical rigour for variable isolation, replication, robustness testing, and external validity | Programme-level decision integration, confidence movement tracking across experiment sequences, and stakeholder communication absent from classical DOE |

## References

* Microsoft. "MLOps maturity model." Azure Architecture Center. <https://learn.microsoft.com/azure/architecture/ai-ml/guide/mlops-maturity-model>
* Zinkevich, Martin. "Rules of Machine Learning: Best Practices for ML Engineering." Google Developers. <https://developers.google.com/machine-learning/guides/rules-of-ml>
* Google. "MLOps: Continuous delivery and automation pipelines in machine learning." Google Cloud Architecture Center. <https://cloud.google.com/architecture/mlops-continuous-delivery-and-automation-pipelines-in-machine-learning>
* AWS. "Machine Learning Lens -- AWS Well-Architected Framework." Amazon Web Services. <https://docs.aws.amazon.com/wellarchitected/latest/machine-learning-lens/machine-learning-lens.html>
* Fisher, Ronald A. *The Design of Experiments*. Oliver and Boyd, 1935. Foundational text for statistical experiment design.
* Taguchi, Genichi. *Introduction to Quality Engineering: Designing Quality into Products and Processes*. Asian Productivity Organization, 1986. Foundational text for robust design methodology.
* Sculley, D.; Holt, Gary; Golovin, Daniel; et al. "Hidden Technical Debt in Machine Learning Systems." *Advances in Neural Information Processing Systems* 28 (2015). <https://papers.nips.cc/paper/5656-hidden-technical-debt-in-machine-learning-systems.pdf>
