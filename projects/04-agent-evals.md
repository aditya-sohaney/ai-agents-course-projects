# Project 4 — Measure What Matters: Agent Evals

> **One-line pitch:** Build a repeatable evaluation harness that tells you whether an agent improved, regressed, became slower, or became more expensive.

## 1. Learning objectives

By the end of this project, you will be able to:

- Convert product requirements into labeled, repeatable agent evaluation cases.
- Measure task success, tool behavior, latency, token usage, and estimated cost.
- Separate deterministic checks, human review, and model-based judgment.
- Compare a baseline and candidate while accounting for nondeterministic results.

## 2. Prerequisites

- Completed Project 2 or 3 and have a runnable agent with visible traces.
- Familiarity with `pytest`, JSONL, basic descriptive statistics, and command-line scripts.
- Ability to mock provider responses and interpret token-usage metadata.

## 3. Background and context

An agent can look impressive in a demo and still fail routinely. Unlike a pure function, an agent may choose different tools or words on repeated runs. Useful evaluation therefore starts from observable task requirements: whether the outcome is correct, whether required or forbidden actions occurred, whether evidence supports the answer, and whether the system stayed within its budget.

No single grader fits every criterion. Exact fields and tool traces can be checked deterministically. Nuanced answer quality may need a small human-reviewed rubric. A model judge can help at scale, but it introduces its own bias and variability, so it must be calibrated against human labels rather than treated as ground truth.

Evaluation is also a change-management practice. A fixed regression set tells you whether a new prompt, model, or tool description improved one slice while breaking another. Latency and cost belong beside quality because an agent that succeeds only after excessive calls may be unusable. Your deliverable is an honest measurement system, not a perfect score.

## 4. The task

1. Select your Project 2 or Project 3 agent as the system under test. Preserve its current behavior as the **baseline**, then make one documented **candidate** change such as a prompt revision, tool-description revision, retrieval setting, or model choice.
2. Create a versioned JSONL evaluation set with at least 30 cases divided into at least five named slices: normal success, ambiguous input, tool failure, adversarial or malformed input, and a domain-specific slice. Project 3 agents must also include grounded-answer and memory/privacy slices.
3. Give every case an ID, input, expected status, deterministic assertions, optional required/forbidden tools, tags, and a short label rationale. Keep at least five cases as a hidden-style holdout that you do not tune against until the end.
4. Implement a provider-independent adapter that runs one case and returns a normalized record: final output, status, tool trace, error, model calls, input/output tokens when available, wall-clock latency, and estimated cost.
5. Implement deterministic graders for schema validity, expected status, required/forbidden tool use, loop-limit compliance, and any exact domain outputs. Add a clearly defined rubric for criteria that need human review.
6. If you use a model judge, require structured judge output, randomize baseline/candidate order, hide system names, and compare at least ten judge decisions with your own labels. Report disagreements; a model judge is optional.
7. Run the baseline and candidate on all 30 cases. Run at least ten representative cases three times per system to estimate consistency. Cache raw outputs so reports can be regenerated without new API calls.
8. Report overall task success rate and slice-level success with numerator/denominator, p50 and p95 latency, average model calls, total tokens when available, and estimated cost per successful task. Do not claim statistical significance from this small set.
9. Define regression gates in a machine-readable config. At minimum: candidate overall success may not drop by more than 3 percentage points, no safety/privacy case may regress, p95 latency may not rise by more than 25%, and estimated cost per successful task may not rise by more than 20% unless justified.
10. Add a command that exits nonzero when a gate fails and use it in a GitHub Actions workflow with mocked or replayed responses. Live API calls must not run on pull requests.
11. Write a one-page decision memo: ship the candidate, revise it, or keep the baseline. Support the decision with results and analyze at least three failures by category.

## 5. Deliverables and submission format

Submit:

- A GitHub repository URL and the release or commit hash to grade.
- The agent adapter, 30-case versioned dataset, graders, cached sanitized runs, report generator, regression-gate config, tests, and offline CI workflow.
- A generated Markdown or HTML report with aggregate and slice metrics for baseline and candidate, consistency results, cost/latency, and links to failure records.
- A README explaining setup, dataset design, label policy, commands, limitations, and actual API spend.
- The one-page decision memo and a 1–2 minute walkthrough of one caught regression.

## 6. Grading rubric

| Category | Weight | Evidence expected |
|---|---:|---|
| Functionality | 30% | Harness runs baseline and candidate through a normalized adapter, caches results, generates the report, and enforces gates. |
| Code quality | 20% | Dataset schema, graders, adapters, metrics, and configuration are modular, reproducible, and provider-aware. |
| Evals/testing | 35% | Cases are representative and labeled; metrics include success, slices, repeatability, cost, and latency; failure analysis is honest. |
| Writeup/documentation | 15% | README and decision memo explain methods, tradeoffs, limitations, results, spend, and the ship/revise decision. |
| **Total** | **100%** | |

## 7. Estimated time and difficulty

- **Estimated time:** 10–14 hours
- **Difficulty:** 3/5
- **Format:** Solo

## 8. Stretch goals

- Build paired bootstrap confidence intervals while clearly documenting the small-sample limitation.
- Add a lightweight local dashboard for filtering failures by slice, tool, and prompt version.
- Detect evaluation-set contamination by hashing cases and tracking which IDs were used during prompt development.

Stretch work does not replace required work and cannot raise a score above 100%.

## 9. Suggested stack

- Python 3.11+
- Your existing raw SDK implementation; LangGraph or a similar framework is allowed but not required
- Pydantic, `pytest`, `pandas` or standard-library statistics
- Jinja2 or a small Python script for report generation
- GitHub Actions using replayed fixtures

**Cheapest path:** Reuse your prior agent, use deterministic graders, cache every live result, run only the required repeated subset, and generate a static Markdown report. You do not need an observability vendor, database, or hosted dashboard.

## 10. Estimated API cost

Expected model spend: **$0.75–$3.50**. This allows 60 baseline/candidate case runs, 60 repeated runs, and a modest development allowance on a low-cost model; omitting the optional model judge lowers cost. Estimate prices in configuration rather than hard-coding them in graders, log usage, stop runs at a user-defined dollar cap, and set a **$4 project alert**. The assignment must remain below approximately **$5**.

