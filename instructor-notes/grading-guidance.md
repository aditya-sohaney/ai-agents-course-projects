# Instructor Grading Guidance

## Default weights

Solo projects use **40% functionality, 20% code quality, 25% evals/testing, and 15% writeup/documentation**. Group projects use **35% functionality, 15% code quality, 25% evals/testing, 15% writeup/documentation, and 10% peer evaluation**. The capstone shifts weight toward demonstrated performance but retains all four required categories.

Grade system behavior and evidence, not provider or framework choice. A small, understandable implementation with strong measurements should outscore a complicated graph whose success is supported only by a curated demo.

Projects 1–4 intentionally use raw provider SDK calls: Project 1 establishes messages, history, prompting, tokens, and cost before Projects 2–4 add tools, reliability, memory, and retrieval. Frameworks become optional in Projects 5–8. When grading Project 2, assume students already know the API and message basics from Project 1; focus feedback on the new action/observation loop.

## Fast, consistent grading workflow

1. **Triage in five minutes:** confirm the submitted commit, scan the README, check for committed secrets, and identify the single documented run command.
2. **Use automation first:** create a clean environment, run tests, then run the evaluation script with the student's cached or low-cost fixture mode. Do not spend API credits merely to discover that imports fail.
3. **Sample three cases:** one normal request, one tool or dependency failure, and one ambiguous or adversarial request. Use the assignment's acceptance cases when provided.
4. **Read evidence, not every line:** inspect contracts, loop bounds, state handling, logs, and the code on the failing path. Use coverage reports as navigation rather than as a grade by themselves.
5. **Leave concise feedback:** cite one strength, one reproducible failure, and the highest-leverage improvement. Reuse the rubric language.

Ask students to include cached example traces with secrets and personal data removed. These traces reduce grading time and make nondeterministic failures discussable. If live provider behavior differs from the submitted results, grade the reproducible artifact and implementation while noting the drift.

## Common failure modes

| Failure mode | What to inspect | Grading response |
|---|---|---|
| Chat history is not actually resent | In Project 1, inspect the messages supplied on turn two and the history-cap behavior. | Deduct functionality where prior turns are unavailable or the system message is trimmed. |
| Token cost is hard-coded or misleading | Inspect usage metadata, configurable prices, session totals, and the “estimate” label. | Deduct functionality or documentation according to whether the calculation or explanation is wrong. |
| A chatbot is labeled an agent | Look for model-selected actions, tool execution, observations, and a termination condition. | Deduct functionality where the required loop is absent. |
| Tool arguments are parsed from prose | Inspect JSON schema validation and malformed-input tests. | Deduct code quality and testing according to impact. |
| The loop can run forever | Look for iteration, token, time, and cost limits. | Treat as a material functionality and safety defect. |
| Tool errors crash or disappear | Run the required failure case; inspect the observation returned to the model. | Deduct functionality; also testing if no case covers it. |
| “Memory” is only the full transcript | Inspect what is stored, retrieved, summarized, expired, and isolated by user/session. | Require the assignment's explicit state and retrieval behavior. |
| RAG claims lack retrieval evidence | Inspect source IDs, chunks, citations, and a no-answer case. | Deduct functionality and evals for unsupported answers. |
| Evals grade style instead of task success | Inspect labels, deterministic checks, and manual-review criteria. | Deduct eval quality even if the dashboard looks polished. |
| Multi-agent design adds no useful separation | Inspect role prompts, contracts, and ablation or comparison evidence. | Deduct architecture/functionality only where requirements are unmet; do not reward agent count. |
| Approval is cosmetic | Try to trigger a side effect before approval or after rejection. | Treat unauthorized execution as a severe functionality defect. |
| Cost claims are guesses | Compare logged calls/tokens/latency with the reported table. | Deduct evals or documentation depending on the missing evidence. |
| Demo-only submission | Run from the documented commit. | Award only evidence that can be reproduced. |

## Group-project calibration

Require teams to record the integration milestone in an issue, tagged commit, or CI run. Individual role labels are starting ownership, not walls: each student should review at least one other module and participate in integration.

Collect peer evaluations privately. Ask each student to allocate 100 contribution points across teammates, including themselves, and provide one concrete artifact for each allocation (commit, review, test, design note, or integration session). Look for corroboration across reports and repository history; do not translate popularity directly into points.

## Cost and access

All required paths should remain below approximately $5 of model usage per student or team. Accept cached model responses for most automated tests; require only a small clearly marked live smoke suite. Students may use either supported provider without penalty. If a provider outage blocks live verification, use recorded traces and retry later rather than grading provider availability.

Never require a paid vector database, observability vendor, or hosting plan. Local JSON/JSONL, SQLite, and in-process vector search are valid. For deployment, free tiers or a locally demo-able CLI satisfy the relevant specification unless the capstone proposal committed to a hosted experience.

## Academic integrity and responsible use

Require attribution for borrowed code, prompts, datasets, and generated assets. A student must be able to explain their state model, tool contract, evaluation labels, and failure-handling decisions. Escalate exposed credentials or unauthorized personal data immediately and ask the student to revoke or remove access according to institutional policy; do not copy secrets into feedback.
