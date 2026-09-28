# Project 8 — Production Agent Capstone

> **One-line pitch:** Design, evaluate, and ship an agent product that solves a real approved problem and is strong enough to discuss in a portfolio or technical interview.

## 1. Learning objectives

By the end of this project, you will be able to:

- Scope an agent around a real user, consequential workflow, and measurable outcome.
- Select tools, memory, retrieval, planning, and approval patterns only where the problem requires them.
- Deploy or package a complete agent experience outside a notebook.
- Evaluate quality, safety, cost, latency, and failure behavior with numerical evidence.
- Communicate architecture, tradeoffs, failures, and redesign ideas to a technical audience.

## 2. Prerequisites

- Completed Projects 1–7 or can demonstrate equivalent skill in LLM APIs, tool use, state/RAG, evals, orchestration, and human approval.
- An instructor-approved proposal, Python development environment, GitHub account, and model-provider API key.
- Ability to use public, synthetic, or explicitly authorized data and to explain the data's provenance.

## 3. Background and context

A capstone is a product argument supported by working evidence. Your repository should make clear who has the problem, why an agent is a suitable interface or control loop, what actions the system may take, and how you know it helps. A large collection of features is less convincing than a focused workflow with reliable behavior.

Production-minded does not mean enterprise infrastructure. It means explicit boundaries: validated tool contracts, durable state where needed, safe handling of data and secrets, bounded loops, approval before consequential actions, observable failures, and a repeatable evaluation. A hosted free-tier app, installable CLI, or browser extension can demonstrate these properties on a student budget.

Your most important artifact is honest engineering judgment. Report the failures alongside the successes. Explain what broke during development, what safeguards you added, and what you would redesign with more time or real usage. Interviewers learn more from measured tradeoffs than from a flawless scripted demo.

## 4. The task

1. Identify a **real problem for a real user or organization** and submit a one-page proposal for instructor approval before implementation. Name the user, current workflow, pain point, agent advantage, non-goals, data source, risks, success metric, and smallest demo-able scope.
2. Do **not** build a toy, tutorial clone, generic “chat with a PDF,” undifferentiated chatbot, or a renamed version of a previous course project. A familiar pattern is acceptable only when it is adapted to a specific user and validated workflow.
3. Choose an appropriate architecture. Your system must include a model-directed loop, at least two useful tools or integrations, validated schemas, bounded execution, structured errors, and sanitized observability. Add RAG, durable memory, multiple agents, or planning only when your proposal justifies them.
4. Put human approval before any consequential or irreversible action, including sending a message, modifying external data, merging code, or creating a public artifact. Enforce permissions and approval in Python outside the model. Simulated integrations are acceptable when real access would create privacy, safety, or cost risk.
5. Make the product **deployed or demo-able outside a notebook** as one of: a hosted web app on a free tier, an installable CLI tool, or a browser extension. Provide a seeded demo mode that a grader can run without private data; API-backed features may require the grader's own key.
6. Build an evaluation set of at least 30 representative cases, including normal, ambiguous, tool-failure, adversarial/prompt-injection, and domain-specific safety or privacy slices. Define each label and preserve a holdout subset.
7. Report, with numbers, task success rate overall and by slice, at least one domain-quality metric, safety/unauthorized-action failures, p50/p95 latency, average model calls, and estimated cost per successful task. Compare against a simple baseline or the pre-agent workflow on at least one key metric.
8. Add unit, contract, and end-to-end tests; a replay or mocked CI mode; and regression gates appropriate to your risk. Cache sanitized evaluation artifacts so the report can be reproduced without spending more credits.
9. Recruit at least two representative users or classmates for structured usability sessions, unless the instructor approves a safety/privacy-based alternative. Record tasks and themes without collecting sensitive personal data; do not present two interviews as statistical proof.
10. Publish the capstone in a **public GitHub repository** containing a polished README, setup and demo instructions, screenshots, an architecture diagram, evaluation method and numerical results, known limitations, responsible-use notes, cost report, and license or explicit reuse terms. Remove all credentials, private data, and copyrighted material you cannot redistribute.
11. Create a 2–3 minute demo video or animated GIF linked prominently from the README. Show the real workflow, one tool or orchestration trace, one handled failure, and the user-visible result—not only slides.
12. Write a 600–900 word reflection answering: What broke? Which assumption was wrong? Which evaluation result changed your design? What would you redesign for ten times the users or higher stakes? What did you deliberately leave out?
13. Pass a release checklist: fresh-clone setup works, demo mode works, tests and eval-report generation run, public links resolve, the architecture diagram matches the code, accessibility basics are addressed, and repository history contains no secrets.

### Seed ideas

Use these as starting points, not as prescribed products:

- A personal research assistant for a specific discipline or recurring decision.
- An email/calendar triage agent with draft-only actions and explicit approval.
- A code-review agent tailored to a real team's conventions and defect history.
- A support agent for a real local business, built with the business's permission and an escalation path.
- A data-pipeline monitoring agent that explains failures and proposes—but does not silently execute—repairs.

Other strong directions are welcome when the user, workflow, agent advantage, and evaluation plan are concrete.

## 5. Deliverables and submission format

Submit:

- The approved proposal and any approved scope changes.
- A **public GitHub repository URL** and a `v1.0` release/tag identifying the graded version.
- A deployed URL, install command, or packaged browser extension plus a seeded demo path. A notebook alone is not accepted.
- Source, tests, `.env.example`, sanitized sample data, evaluation dataset, replayable runs, regression configuration, and generated results.
- A quality README with a one-sentence value proposition, target user, screenshots, architecture diagram, quick start, usage, safety/approval model, evals with numbers, cost, limitations, and attribution.
- A linked 2–3 minute demo video or GIF, the 600–900 word reflection, and a one-page portfolio summary suitable for an interview handout.

## 6. Grading rubric

| Category | Weight | Evidence expected |
|---|---:|---|
| Functionality | 35% | The approved real workflow works end to end in a deployed or packaged product; tools, bounds, approvals, and failure paths behave as specified. |
| Code quality | 20% | Architecture and contracts are coherent; setup is reproducible; secrets/data are safe; logs, CI, and dependencies are production-minded. |
| Evals/testing | 30% | At least 30 cases, automated tests, holdout/regression evidence, baseline comparison, and numerical quality/safety/cost/latency results support the claims. |
| Writeup/documentation | 15% | Public README, diagram, demo, reflection, portfolio summary, provenance, limitations, and actual spend are clear, candid, and polished. |
| **Total** | **100%** | |

## 7. Estimated time and difficulty

- **Estimated time:** 35–50 hours across four to five weeks
- **Difficulty:** 5/5
- **Format:** Solo

## 8. Stretch goals

- Add an opt-in feedback mechanism and show how feedback becomes a versioned evaluation case.
- Run a small architecture ablation to justify one complex component with quality, cost, and latency evidence.
- Add accessibility or internationalization improvements validated by a representative user.

Stretch work does not replace required work and cannot raise a score above 100%.

## 9. Suggested stack

- Python 3.11+ with the OpenAI or Anthropic SDK
- Plain Python orchestration or, optionally, LangGraph or a similar framework
- Pydantic, SQLite/PostgreSQL free tier, `pytest`, and structured logging
- Typer/Rich for an installable CLI; FastAPI/Streamlit for a web app; or a thin browser-extension front end calling a Python service
- GitHub Actions in replay/mock mode; Mermaid, D2, or another text-based diagram tool
- Vercel, Render, Railway, or another suitable free tier when hosting is useful

**Cheapest path:** Ship an installable Python CLI with SQLite, local retrieval, a low-cost text model, cached eval runs, GitHub Actions replay mode, and a screen-recorded demo. This satisfies deployment/demo requirements without paid hosting, a GPU, or any infrastructure purchase.

## 10. Estimated API cost

Expected model spend: **$2.00–$4.75**. This assumes a focused workflow, one 30-case final evaluation, a small repeated subset, two user sessions, and disciplined development on a low-cost model. Use cached fixtures, per-run call/token limits, a configurable dollar cutoff, and a provider spending alert at **$4.50**. If your proposed architecture cannot be demonstrated and evaluated below approximately **$5**, reduce scope or use more deterministic/local components before seeking approval.
