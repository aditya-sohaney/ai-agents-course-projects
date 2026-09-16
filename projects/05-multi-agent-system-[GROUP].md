# Project 5 — Research Brief Studio `[GROUP]`

> **One-line pitch:** As a team, build a small system of specialist agents that turns a document packet into a cited research brief—and prove that the extra agents earn their complexity.

## 1. Learning objectives

By the end of this project, you will be able to:

- Assign bounded responsibilities to specialist agents and define their handoff contracts.
- Coordinate parallel and sequential work without sharing hidden mutable state.
- Detect unsupported claims and disagreements before synthesis.
- Compare a multi-agent design with a simpler single-agent baseline.
- Integrate independently developed modules through contract and end-to-end tests.

## 2. Prerequisites

- Completed Projects 3 and 4: local-document RAG, state, trace logging, labeled evals, and regression metrics.
- Teams of three or four with a shared Git workflow and issue board.
- A model-provider API key per team or an instructor-provided key with a strict budget. No paid search, vector database, or hosting service is required.

## 3. Background and context

Multiple agents are useful when a task has genuinely different responsibilities, information boundaries, or checks that can run independently. They are not automatically better than one well-instructed agent. Every handoff adds latency, cost, and another place where context can be lost or malformed.

A strong multi-agent system makes roles and messages explicit. In this project, one specialist gathers evidence, another challenges claims, and another synthesizes the final brief. They exchange validated artifacts—not free-form claims about what another agent “thought.” The orchestrator owns shared state and termination; specialists cannot invoke one another implicitly.

Team structure should mirror system structure without creating isolated silos. Each student owns a primary module, but shared schemas and an early integration test force modules to meet while changes are still affordable. Your evaluation will include a single-agent baseline so your team can decide where specialization helped and where it did not.

## 4. The task

1. Form a team of three or four and choose an instructor-approved research domain that can be supported by a packet of 10–20 public, synthetic, or authorized documents. Define five representative research questions before implementation.
2. Build a **single-agent baseline** that retrieves from the packet and writes a brief. Preserve its prompt and evaluation results.
3. Build a multi-agent system with at least these roles: **Evidence Scout** retrieves and extracts candidate evidence; **Claim Reviewer** verifies citations, identifies conflicts, and requests missing support; **Brief Writer** synthesizes only accepted evidence. A deterministic Python orchestrator routes artifacts and owns loop limits.
4. Define versioned structured contracts for `ResearchRequest`, `EvidenceItem`, `ReviewDecision`, and `FinalBrief`. Every evidence item must include a stable source/chunk ID and a short relevant excerpt; every review decision must identify the claim and verdict.
5. Use at least one parallel step and one sequential dependency. Cap the workflow at eight model calls and two revision rounds per question; expose a clear stop reason.
6. Produce a 600–900 word brief with an executive summary, findings, disagreements or uncertainty, and a source list. Verify programmatically that every inline citation refers to evidence present in the run.
7. Handle a specialist timeout, malformed handoff, empty retrieval result, and conflicting sources. The orchestrator must retry only safe/idempotent work, route repair deliberately, and return a partial-result status when completion is impossible.
8. Log a trace with run ID, agent role, artifact IDs, status, latency, usage/cost, retry count, and stop reason. Do not log hidden reasoning, API secrets, or unredacted sensitive data.
9. Create at least 20 labeled evaluation cases across answerable, insufficient-evidence, conflicting-source, malformed-input, and injected-document slices. Compare baseline and multi-agent task success, citation validity, unsupported-claim rate, p95 latency, and estimated cost.
10. Write a short architecture decision record answering: Which handoff improved quality? Which added cost without clear benefit? What would you simplify?
11. **Role split:** Student A owns document ingestion, retrieval, and Evidence Scout; Student B owns orchestration, schemas, and recovery; Student C owns evals, observability, and Claim Reviewer; Student D owns Brief Writer, CLI or web demo, and documentation. In a three-person team, Student A also owns the demo. Each student must review at least one module they do not own.
12. **Mid-project integration milestone—end of week 1:** merge skeleton implementations for every role; make all modules pass the same contract-test suite; and complete one end-to-end run with deterministic fixtures in CI. Tag this commit `integration-v1` and attach the passing run to the issue board. Missing the milestone must be documented in the final reflection.
13. Complete confidential peer evaluations allocating 100 contribution points across all team members, including yourself, with at least one repository artifact supporting each allocation.

## 5. Deliverables and submission format

Submit:

- One team GitHub repository URL and the `v1.0` release tag or commit hash.
- Source, schemas, document provenance, tests, `.env.example`, baseline, multi-agent system, and replayable sanitized traces.
- The tagged `integration-v1` milestone with CI evidence and the final issue board export or screenshot.
- A 20-case dataset and comparative report with quality, citation, unsupported-claim, latency, and cost metrics.
- A README with setup, architecture and sequence diagrams, role ownership, recovery policy, results, limitations, and actual API spend.
- A 2–3 minute team demo in which each member explains their owned module and the system handles one conflicting-source case.
- One architecture decision record and one confidential peer-evaluation form per student, submitted through the course LMS rather than committed publicly.

## 6. Grading rubric

| Category | Weight | Evidence expected |
|---|---:|---|
| Functionality | 35% | Specialists perform distinct roles, validated handoffs drive an end-to-end cited brief, and the orchestrator handles required failures within bounds. |
| Code quality | 15% | Shared contracts, module boundaries, review history, safe configuration, traces, and setup support parallel development and maintenance. |
| Evals/testing | 25% | Contract and integration tests pass; 20 cases compare multi-agent and baseline quality, citation validity, cost, and latency by slice. |
| Writeup/documentation | 15% | README, diagrams, milestone evidence, ADR, demo, limitations, roles, and spend accurately explain the result. |
| Peer evaluation | 10% | Specific, corroborated evidence shows contribution, cross-review, communication, and integration work consistent with the agreed role. |
| **Total** | **100%** | |

## 7. Estimated time and difficulty

- **Estimated time:** 20–26 hours per team member across two weeks
- **Difficulty:** 4/5
- **Format:** Group of 3–4

## 8. Stretch goals

- Let the Claim Reviewer request one targeted retrieval query and measure whether it improves citation coverage.
- Add a deterministic redundancy check that detects substantially duplicated findings before synthesis.
- Run an ablation that removes one specialist and quantify the effect on quality, latency, and cost.

Stretch work does not replace required work and cannot raise a score above 100%.

## 9. Suggested stack

- Python 3.11+
- Direct provider SDK or, optionally, LangGraph or a similar orchestration framework
- Pydantic for contracts; SQLite or JSONL for run state and traces
- Local TF-IDF/BM25 or locally stored embeddings
- `pytest` contract tests and GitHub Actions with replayed fixtures
- Optional Streamlit or FastAPI demo

**Cheapest path:** Use instructor-provided Markdown packets, local lexical retrieval, a plain Python orchestrator, JSONL traces, a terminal demo, and cached responses for CI. Use a low-cost model only for required live runs; no paid search or hosting is needed.

## 10. Estimated API cost

Expected model spend: **$1.50–$4.50 per team**. This assumes one 20-case baseline run, one multi-agent run, limited development sampling, short evidence excerpts, and strict call/output caps on a low-cost model. Share a team ledger, cache every completed role artifact, stop automatically at **$4.50**, and use replay mode for integration tests. The assignment must remain below approximately **$5 total per team**.

