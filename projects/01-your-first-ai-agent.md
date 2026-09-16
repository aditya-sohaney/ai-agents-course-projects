# Project 1 — Your First AI Agent

> **One-line pitch:** Build the smallest useful agent: a raw model call in a visible loop that can choose one calculator tool, observe its result, and answer the user.

## 1. Learning objectives

By the end of this project, you will be able to:

- Trace the agent loop: **prompt → tool call → observation → response**.
- Define one tool with a machine-readable input contract.
- Execute a model-requested action in Python and return the observation to the model.
- Bound an agent loop and explain why an ordinary chat completion is not automatically an agent.

## 2. Prerequisites

- Comfortable writing and running a small Python program.
- Basic functions, dictionaries, loops, exceptions, and command-line input.
- Git and a model-provider API key. No previous agent or machine-learning experience is assumed.

## 3. Background and context

An agent is not magic and it is not defined by a framework. In this project, an agent is a model participating in a loop. The model receives a user request and a description of an available action. It may request that action, your Python code executes it, and the resulting observation goes back to the model so it can decide what to say next.

The important boundary is between deciding and doing. The model may propose calculator arguments, but ordinary Python code validates and executes them. The result is then represented as a message rather than silently inserted into prose. Keeping those steps separate makes the system inspectable and testable.

Even this tiny loop needs a stop condition. Models can repeat a request, produce invalid arguments, or answer without using a tool. You will cap the loop and print a compact trace so you can see every decision. The goal is understanding the mechanism, not hiding it behind abstractions.

## 4. The task

1. Create a Python 3.11+ command-line program that accepts one user question per run.
2. Call either the OpenAI or Anthropic API directly through its official Python SDK. **Do not use LangChain, LangGraph, CrewAI, an agent SDK, or another orchestration framework.**
3. Expose exactly one tool named `calculator`. Give it a JSON schema with a required string field named `expression` and a clear description.
4. Implement `calculator(expression)` without Python `eval`. Support parentheses, decimal numbers, and `+`, `-`, `*`, and `/`; reject every other syntax. Return either a numeric result or a structured error observation.
5. Implement the loop explicitly: send messages and the tool definition to the model; inspect the response; if it requests the calculator, validate and execute the call; append the tool result as an observation; and call the model again. Print the final response when no tool is requested.
6. Stop after at most three model calls. If the limit is reached, return a clear user-facing failure instead of continuing.
7. Print or save a trace containing the iteration number, requested tool name, validated arguments, tool result or error, and final stop reason. Do not log the API key or full authorization headers.
8. Include at least six automated tests: four calculator unit tests, one invalid-expression test, and one mocked agent-loop test proving that an observation is returned to the model before the final answer.
9. Demonstrate these acceptance cases: `What is (17 * 4) + 9?`, one non-arithmetic question the model can answer directly, and one malicious or unsupported calculator expression that is rejected safely.
10. Read credentials from an environment variable, commit a `.env.example` with variable names only, and document how to set a maximum output-token limit.

## 5. Deliverables and submission format

Submit:

- A GitHub repository URL and the commit hash to grade.
- Source code with a clear entry point such as `python -m agent`.
- `requirements.txt` or `pyproject.toml`, `.env.example`, and `pytest` tests.
- A README containing setup, a five-line explanation of the loop, usage examples, the three acceptance-case transcripts, known limitations, and actual API spend.
- A 30–60 second terminal recording or GIF showing a calculator call and the returned observation. A link in the README is sufficient.

## 6. Grading rubric

| Category | Weight | Evidence expected |
|---|---:|---|
| Functionality | 40% | A raw-SDK loop completes the required trace with exactly one safe calculator tool, valid observations, and a hard iteration limit. |
| Code quality | 20% | Decision, validation, execution, and display concerns are readable and separated; secrets are safe; setup is reproducible. |
| Evals/testing | 25% | The six required tests and three acceptance cases run; failures identify the responsible step. |
| Writeup/documentation | 15% | README accurately explains the loop, setup, limitations, transcripts, and actual spend. |
| **Total** | **100%** | |

## 7. Estimated time and difficulty

- **Estimated time:** 4–6 hours
- **Difficulty:** 1/5
- **Format:** Solo

## 8. Stretch goals

- Add exponentiation while preserving an explicit operator allowlist and resource limits.
- Render the trace as a small Mermaid sequence diagram generated from recorded events.
- Add a deterministic replay mode that runs from a saved model-response fixture without an API key.

Stretch work does not replace required work and cannot raise a score above 100%.

## 9. Suggested stack

- Python 3.11+
- OpenAI or Anthropic Python SDK, called directly
- `pytest`
- Standard-library `ast`, `decimal`, `json`, and `logging` as useful

**Cheapest path:** Use a provider's low-cost text model, short prompts, a three-call cap, mocked tests, and only three live acceptance runs. No hosting, database, GPU, or paid calculator service is needed.

## 10. Estimated API cost

Expected model spend: **$0.05–$0.50**. This assumes fewer than 40 short development calls, three final live acceptance cases, and a modest output-token cap on a low-cost text model. Mock the model in automated tests, log per-run usage when the SDK returns it, and set yourself a **$2 project alert**. The assignment must remain below approximately **$5**.

