# Foglight Interaction Sequence

A typical Foglight session follows six stages. The sequence is guidance, not a rigid script. The Orchestrator can loop back to earlier stages when new context surfaces, skip stages that prior context already covers, or hand off to another HVE Core agent when that is the better next action. This mirrors the DT Coach pattern, which allows non-linear navigation when evidence is already in hand.

## The six stages

| Stage                        | Orchestrator behavior                                                                                                                              | User outcome                                                                                     |
|------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------|
| 1. Understand the context    | Gather bounded project context from repo documents, optional read-only backlog sources, and a short user statement of the situation.               | A shared, explicit framing of the current project state and the pending decision.                |
| 2. Frame the capability      | Explain what Foglight can and cannot do for the situation, and where it defers to other HVE Core agents.                                           | Clear expectations and no false promises of generic research, planning, or implementation help.  |
| 3. Offer areas of expertise  | Surface the Foglight expertise lanes that apply, for example scope framing, evidence maturity, readiness, operations, learning loops, or change control. | A short menu of relevant lanes rather than a flat list of every capability.                      |
| 4. Prescribe relevant tools  | Recommend the smallest useful Foglight artifact for the chosen lane, with the rationale for why it fits the decision and evidence posture.          | A focused recommendation tied to a named decision, not a generic checklist.                      |
| 5. Templatize for context    | Generate a context-specific draft by populating the artifact template from repo evidence, backlog signals, and the user's framing.                  | A working draft that reflects the actual project, not a blank template.                           |
| 6. Co-author interactively   | Work the artifact with the user, prompt for missing evidence, refine confidence and shelf-life entries, and prepare handoffs when needed.           | A usable artifact and a clear next action, including any handoff to an adjacent HVE Core agent.   |

## Loop-back and defer rules

* Loop back to Stage 1 whenever the crew introduces context that changes the pending decision.
* Skip a stage when prior context already satisfies its outcome; state that you are skipping it and why.
* At any stage that produces insufficient signal to continue, name the gap and offer three options: defer, gather more evidence, or hand off.
* Prefer a handoff over forcing an artifact when the next best action is research, planning, backlog operations, RAI, security, or implementation.

## Dispatch within a stage

* Stages 3 and 4 use the capability registry to choose a lane and a single artifact.
* Stages 5 and 6 dispatch the capability's subagent to generate or update the artifact, then return to co-authoring.
* Persist state after each stage that changes the pending decision, the chosen capability, or an artifact's status, following the state schema.

## Handoff points

Declare handoffs through the agent's `handoffs` frontmatter and offer them when the next action leaves Foglight's lane.

* To `Task Researcher` when repo evidence is incomplete and discovery is needed.
* To `Task Planner` when Foglight identifies follow-on work that belongs in a plan or backlog.
* To `RAI Planner` when model-behavior, evaluation-coverage, or operator-workflow concerns need a responsible-AI assessment.
* To `Code Reviewer`, `Security Planner`, or `SSSC Planner` when readiness, QA, or supply-chain evidence should flow into those reviews.
