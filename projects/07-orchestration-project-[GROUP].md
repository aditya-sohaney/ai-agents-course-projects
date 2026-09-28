# Project 7 — Human-in-the-Loop Operations Orchestrator `[GROUP]`

> **One-line pitch:** Build a resumable service-desk workflow that plans work, delegates bounded tasks, and pauses for human approval before any consequential action.

## 1. Learning objectives

By the end of this project, you will be able to:

- Decompose a request into a dependency-aware plan and execute only ready steps.
- Represent a long-running agent workflow as durable, resumable state.
- Place enforceable human approval gates around consequential tools.
- Apply retry, idempotency, timeout, and compensation policies to tool execution.
- Coordinate team-owned modules through shared schemas and integration tests.

## 2. Prerequisites

- Completed Projects 5 and 6: evaluation gates, multi-agent roles, structured handoffs, and observability.
- Teams of three or four comfortable with SQLite, state machines, test doubles, and GitHub pull requests.
- A model-provider API key per team. All service-desk systems and side effects in this assignment are simulated locally.

## 3. Background and context

An orchestration system must decide not only what should happen, but what may happen now. A plan can contain dependencies, actions safe to retry, actions that need a person, and actions that must never execute twice. Treating all tool calls alike is dangerous when a model can trigger an external side effect.

Human-in-the-loop design is more than an “Approve” button. The system must persist the exact proposed action, show the reviewer enough context to decide, bind approval to that immutable action, and refuse execution after rejection or unauthorized modification. A paused workflow should survive a process restart and resume without repeating completed work.

Operational reliability depends on explicit state transitions. Timeouts, temporary failures, duplicate delivery, and partial completion are normal. You will use local simulated integrations so you can test these conditions aggressively without contacting a real customer or changing a real ticket.

## 4. The task

1. Build a **Service Desk Orchestrator** for fictional incoming requests such as account-access help, facilities issues, or policy questions. Use synthetic records only; do not connect to real email, calendars, or ticket systems.
2. Accept a structured request and produce a validated plan containing step IDs, descriptions, dependencies, responsible worker role, tool, risk level, and completion criterion. Reject cyclic plans and cap plans at eight steps.
3. Implement at least three bounded worker roles: **Triage Worker** classifies and gathers facts; **Resolution Worker** searches a local policy/solution base and drafts action; **Communications Worker** drafts the requester update. A deterministic orchestrator, not a worker, owns plan state and permissions.
4. Implement at least four simulated tools: `search_knowledge_base`, `create_or_update_ticket`, `draft_message`, and `send_message`. `send_message` and any ticket closure or privilege-changing action are consequential and must require approval; safe read-only work may proceed automatically.
5. Persist workflow state in SQLite with explicit states such as `PLANNING`, `RUNNING`, `WAITING_FOR_APPROVAL`, `REJECTED`, `FAILED`, and `COMPLETED`. Provide a CLI or small web interface to list, inspect, approve, reject, and resume runs after restarting the process.
6. Create an approval record containing run ID, step ID, immutable action hash, human-readable preview, risk reason, requester, approver, decision, timestamp, and optional feedback. Execution must verify the hash and authorization. Rejection must prevent the action and route feedback to a bounded re-plan or termination.
7. Give every side-effecting call an idempotency key. Implement timeouts, at most two retries with backoff for declared transient failures, no automatic retry for ambiguous side effects, and a visible partial-failure state.
8. Enforce permissions in Python outside the model. Include tests proving that prompt injection, a forged approval ID, a stale approval after action edits, and direct tool invocation cannot bypass the gate.
9. Log structured state transitions, plan revisions, tool attempts, approvals, latency, token usage/cost, errors, and final outcome. Redact synthetic secrets and never store hidden model reasoning.
10. Build at least 24 evaluation scenarios covering normal completion, dependency ordering, required approval, rejection/re-plan, restart/resume, transient failure, ambiguous side effect, duplicate request, malicious input, and budget exhaustion. Report task success, unauthorized-action count, duplicate-side-effect count, recovery success, p95 latency excluding human wait time, and model cost.
11. **Role split:** Student A owns simulated integrations, idempotency, and failure injection; Student B owns planning, orchestration, and persistence; Student C owns security tests, evals, and observability; Student D owns the approval UI/CLI, demo, and operator documentation. In a three-person team, Student D's responsibilities are split between A and C. Each member must review another member's safety-critical code.
12. **Mid-project integration milestone—end of week 2:** all tool and state schemas are frozen as `v1`; contract tests pass for every module; a deterministic run pauses at approval, survives a restart, resumes, and completes in CI. Tag the evidence `integration-v1` before adding final prompts or visual polish.
13. Complete confidential peer evaluations allocating 100 contribution points across all members, including yourself, and cite commits, reviews, tests, documents, or integration work as evidence.

## 5. Deliverables and submission format

Submit:

- One team GitHub repository URL and the `v1.0` release tag or commit hash.
- Source, migrations, synthetic fixtures, shared schemas, failure controls, tests, `.env.example`, and replayable traces.
- The `integration-v1` tag with passing CI evidence and a threat model covering assets, trust boundaries, likely abuse, and mitigations.
- A 24-scenario evaluation set and report with success, safety, duplicate-action, recovery, latency, and cost metrics.
- A README with setup, architecture/state diagrams, role ownership, approval and retry policies, operator runbook, limitations, and actual spend.
- A 3–4 minute team demo showing plan execution, a restart while paused, rejected or modified action handling, authorized completion, and failure recovery.
- One confidential peer-evaluation form per student, submitted through the course LMS rather than committed.

## 6. Grading rubric

| Category | Weight | Evidence expected |
|---|---:|---|
| Functionality | 35% | Plans respect dependencies; durable runs pause/resume; approval gates cannot be bypassed; side effects and failures follow policy. |
| Code quality | 15% | State machine, permissions, schemas, idempotency, modules, migrations, and safe configuration are explicit and maintainable. |
| Evals/testing | 25% | Contract, integration, restart, adversarial, and 24-scenario evidence measures success, safety, recovery, latency, and cost. |
| Writeup/documentation | 15% | README, diagrams, threat model, milestone, runbook, demo, roles, limitations, and spend support operation and review. |
| Peer evaluation | 10% | Corroborated evidence shows meaningful ownership, safety review, collaboration, and integration contribution. |
| **Total** | **100%** | |

## 7. Estimated time and difficulty

- **Estimated time:** 24–32 hours per team member across three weeks
- **Difficulty:** 5/5
- **Format:** Group of 3–4

## 8. Stretch goals

- Add expiring approvals and escalation when a run waits beyond a configurable service-level target.
- Add a dry-run mode that renders the planned side effects and compares them with the final execution ledger.
- Use property-based state-machine testing to explore unexpected transition sequences.

Stretch work does not replace required work and cannot raise a score above 100%.

## 9. Suggested stack

- Python 3.11+
- Plain Python state machine or, optionally, LangGraph or a similar framework
- Pydantic, SQLite, and a migration tool or versioned SQL
- `pytest`, failure-injection fixtures, and GitHub Actions replay mode
- Typer/Rich for CLI or FastAPI/Streamlit for a local approval UI

**Cheapest path:** Simulate every integration with SQLite and local files, use a plain Python orchestrator and terminal approval UI, mock model calls in CI, and reserve a low-cost model for the final scenario run. Deployment is optional and no paid service is needed.

## 10. Estimated API cost

Expected model spend: **$1.50–$4.50 per team**. This assumes bounded plans, one required 24-scenario live run, cached role outputs, and a low-cost text model; all retry and restart tests should use fixtures. Maintain a shared ledger, cap per-run model calls, and stop automatically at **$4.50**. The assignment must remain below approximately **$5 total per team**.
