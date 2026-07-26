---
title: Phase Gate Dashboard Summary Template
description: Gate-decision input consolidating each dimension's level, trend, evidence, and recommendation.
---

## Phase Gate Dashboard Summary Template

Use this template at phase gates to consolidate the dashboard into a gate decision input. Summarize the recent trend using the dimension's declared refresh cadence and give a per-dimension recommendation.

Decision: <the gate decision under consideration>
Confidence legend: red is Low, amber is Medium, green is High.

| Dimension               | Level                | Trend (recent cycles) | Key evidence | Gate recommendation             |
|-------------------------|----------------------|-----------------------|--------------|---------------------------------|
| Problem Confidence      | Low / Medium / High  | up / down / flat      | Summary      | Proceed / Hold / Conditional    |
| Data Confidence         | Low / Medium / High  |                       |              |                                 |
| Feasibility Confidence  | Low / Medium / High  |                       |              |                                 |
| Signal Confidence       | Low / Medium / High  |                       |              |                                 |
| Applicability Confidence | Low / Medium / High  |                       |              |                                 |
| Operational Confidence  | Low / Medium / High  |                       |              |                                 |

Gate logic:

* Proceed requires all six dimensions at Medium or above with documented evidence.
* Any dimension at Low blocks the gate; attach a specific action plan for that dimension.
* A Medium dimension may support a conditional proceed when the condition names the evidence still owed and when it is due.

Reason over the dimensions individually. Do not average them into a single gate score.
