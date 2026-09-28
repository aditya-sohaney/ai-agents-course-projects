# Project 3 — Reliable Tool-Use Agent

> **One-line pitch:** Turn the tiny loop into a dependable single agent that chooses among a few local tools and fails clearly when a tool cannot complete its job.

## 1. Learning objectives

By the end of this project, you will be able to:

- Design unambiguous tool names, descriptions, and JSON schemas.
- Validate model-generated arguments before calling Python functions.
- Produce a structured final response instead of parsing free-form prose.
- Distinguish tool errors, model errors, and user-input errors and handle each deliberately.

## 2. Prerequisites

- Completed Project 2 or can independently implement its raw-SDK tool-calling loop.
- Familiarity with Python type hints, exceptions, JSON, and `pytest` fixtures or mocks.
- A model-provider API key. No database or external paid service is required.

## 3. Background and context

Once an agent has multiple tools, descriptions and schemas become part of its program. Two overlapping tool descriptions can make selection unpredictable. Loose argument types push error handling deep into business logic. A robust agent presents a small, distinct action space and treats model output as untrusted input.

Structured outputs also create a dependable boundary between the agent and the software that consumes it. Instead of scraping sentences to discover whether a request succeeded, downstream code should receive a validated object such as `{status, answer, tools_used, error}`. The user can still see friendly prose, but the program gets a stable contract.

Reliability includes expected failure. A file can be missing, an argument can fall outside its allowed range, or a tool can time out. Your agent should turn that event into an observation, give the model one bounded opportunity to recover, and return an honest status. This project remains stateless across user requests so you can focus on tool behavior before adding memory.

## 4. The task

1. Build a Python command-line **Campus Utility Agent** using direct OpenAI or Anthropic SDK calls. Agent frameworks remain prohibited.
2. Give the single agent exactly three local, deterministic tools: `calculator(expression)`, `convert_units(value, from_unit, to_unit)`, and `lookup_policy(topic)` over a supplied or self-authored JSON file containing at least eight fictional campus policies. Do not call the public web.
3. Define strict schemas for every tool. Use enums where the domain is closed, reject extra fields, validate types and ranges in Python, and return a common result envelope: `{ok, data, error_code, message}`.
4. Require the model to return a final structured object containing `status` (`answered`, `needs_clarification`, or `failed`), `answer`, `tools_used`, and `error`. Validate this object before displaying it; make one repair attempt, then fail clearly.
5. Preserve messages only within the current request's agent loop. Starting a new command must create a fresh session with no memory of earlier commands.
6. Cap each request at five model calls and three tool executions. Detect an unknown tool, repeated identical calls, invalid arguments, and unavailable policy data.
7. Inject at least one controlled tool failure through a flag or test double. Return the failure as an observation and let the agent either use another valid path, ask for clarification, or report that it could not finish. It must never invent a tool result.
8. Emit structured JSONL trace events with request ID, event type, tool name, latency, success flag, and sanitized error code. Keep user-visible output separate from diagnostics.
9. Add at least 12 automated tests: one success and one failure for each tool, three schema-validation cases, two loop-control cases, and one end-to-end mocked conversation.
10. Publish a six-case acceptance set containing at least one request for each tool, one request requiring two tools, one ambiguous request, and one forced failure. Record expected status and required tool usage for each case.

## 5. Deliverables and submission format

Submit:

- A GitHub repository URL and immutable commit hash.
- Runnable source, typed tool schemas, the fictional policy dataset, tests, `.env.example`, and dependency metadata.
- `evals/acceptance.jsonl` plus a command that runs all six cases and writes a summary without exposing secrets.
- A README with setup, architecture, all tool contracts, loop limits, error taxonomy, sample output, limitations, and measured API spend.
- A 1–2 minute terminal demo showing a two-tool request and the controlled failure path.

## 6. Grading rubric

| Category | Weight | Evidence expected |
|---|---:|---|
| Functionality | 40% | The single stateless agent selects among three tools, emits the final response contract, respects bounds, and handles the forced failure honestly. |
| Code quality | 20% | Schemas are precise; validation, tools, orchestration, and presentation are separated; traces and secrets are handled safely. |
| Evals/testing | 25% | Twelve required tests pass and the six-case acceptance report compares actual status/tool use with expectations. |
| Writeup/documentation | 15% | README documents setup, architecture, contracts, failures, limits, results, and spend accurately. |
| **Total** | **100%** | |

## 7. Estimated time and difficulty

- **Estimated time:** 7–9 hours
- **Difficulty:** 2/5
- **Format:** Solo

## 8. Stretch goals

- Add property-based tests that generate valid and invalid unit conversions.
- Let the user request an explanation of which tool was chosen without revealing hidden reasoning.
- Compare two alternative tool descriptions on the acceptance set and report selection accuracy.

Stretch work does not replace required work and cannot raise a score above 100%.

## 9. Suggested stack

- Python 3.11+
- OpenAI or Anthropic Python SDK, called directly
- Pydantic or `jsonschema` for contracts
- `pytest` plus standard-library JSONL logging

**Cheapest path:** Keep every tool local, use JSON instead of a service or database, run most tests with saved responses, and reserve live API calls for the six acceptance cases. No deployment or paid integration is needed.

## 10. Estimated API cost

Expected model spend: **$0.10–$0.75**. This assumes roughly 60 short development calls and one six-case live acceptance run using a low-cost text model. Track returned token usage and latency per call, impose output and loop caps, and set a **$2 project alert**. The assignment must remain below approximately **$5**.
