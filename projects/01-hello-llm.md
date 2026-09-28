# Project 1 — Hello, LLM

> **One-line pitch:** Build a friendly command-line chatbot that calls a language model directly, remembers the current conversation, and shows what each session costs.

## 1. Learning objectives

By the end of this project, you will be able to:

- Explain what an LLM API call sends and receives.
- Use `system`, `user`, and `assistant` message roles correctly.
- Explain why a program must resend conversation history on every turn.
- Compare how system prompts and temperature settings change model behavior.
- Read token usage, estimate session cost, and handle common API failures safely.

## 2. Prerequisites

- Comfortable writing and running a small Python program.
- Basic functions, lists, dictionaries, loops, exceptions, and command-line input.
- Git and a model-provider account with a small amount of API credit. No LLM or agent experience is assumed.

## 3. Background and context

A language model API is a request-and-response service. Your Python program sends a model name and a list of messages, then receives generated text plus metadata such as token usage. The model does not automatically see your terminal, files, earlier program runs, or secrets; it sees only the information included in the current request.

Chat messages have roles. A `system` message establishes the assistant's job and constraints, a `user` message contains a person's request, and an `assistant` message records the model's prior reply. Most chat APIs are stateless, so your program creates the experience of memory by keeping earlier messages in a Python list and resending that list on the next turn.

Generation settings influence behavior and cost. Temperature can make supported models more consistent or more varied, while longer conversations use more input tokens because history is resent. You will compare configurations, display usage, and keep a running cost estimate. This is a chatbot, not yet an agent: it has no tools and cannot take actions outside the conversation.

## 4. The task

1. Create a Python 3.11+ command-line program using the official OpenAI or Anthropic Python SDK directly. Do not use an agent or chat framework.
2. Load the API key from a `.env` file or environment variable. Include `.env` in `.gitignore`, commit a `.env.example` containing variable names only, and never print or log the key.
3. Make one single-turn API call from Python and print the model's text response. Keep this path available through a command such as `python -m chatbot --once "Hello"`.
4. Add an interactive chat loop. Store the system message plus each user and assistant message in a list, and resend the complete list on each turn so the model can refer to an earlier detail. Support `quit` and `exit` without making another API call.
5. Add a configurable system prompt that gives the bot a specific persona or job, such as “study buddy for an introductory statistics class.” Display the active persona when the session begins.
6. Run and document three configurations, each with a different system prompt and temperature. Ask the same two prompts in every configuration, save the outputs, and write a 200–300 word comparison of specificity, tone, consistency, and variation. If your chosen model does not expose temperature, choose one that does for this experiment or document the closest supported sampling control.
7. Catch and explain an invalid API key or authentication failure, a rate-limit response, and a network/connection failure. Return a helpful message and allow the user to retry or exit; do not show a Python traceback during normal use.
8. After each response, print input tokens, output tokens, and the running session total. Calculate estimated cost from configurable per-million-token input and output prices, label it as an estimate, and show at least six decimal places for small sessions.
9. Cap the stored history by a documented maximum number of messages or tokens. When the cap is reached, warn the user and remove the oldest user/assistant pair without removing the system message.
10. Add at least eight automated tests covering message construction, history across two turns, history trimming, clean exit, cost calculation, and the three required error categories. Mock the SDK so tests run without an API key.

## 5. Deliverables and submission format

Submit:

- A GitHub repository URL and the commit hash to grade.
- Runnable source with both single-turn and interactive modes, pinned dependencies, `.gitignore`, and `.env.example`.
- At least eight `pytest` tests that use a mocked model client and require no network access.
- A README with setup, commands, message-role explanation, history behavior, supported error cases, model and price assumptions, limitations, and measured API spend.
- The three-configuration output record and 200–300 word comparison.
- A `sample_run.md` transcript showing an earlier fact recalled on a later turn, token usage, estimated session cost, and a handled error.

## 6. Grading rubric

| Category | Weight | Evidence expected |
|---|---:|---|
| Functionality | 40% | Single-turn and chat modes work; history is resent and bounded; persona, clean exit, usage/cost display, and three error paths behave as required. |
| Code quality | 20% | API access, message state, configuration, cost calculation, and presentation are separated; secrets are safe and setup is reproducible. |
| Evals/testing | 25% | Eight or more offline tests cover state, trimming, cost, exit, and authentication/rate/network failures. |
| Writeup/documentation | 15% | README, sample run, three configurations, comparison, limitations, and actual spend clearly explain the observed behavior. |
| **Total** | **100%** | |

## 7. Estimated time and difficulty

- **Estimated time:** 3–5 hours
- **Difficulty:** 1/5
- **Format:** Solo

## 8. Stretch goals

- Add a command that saves and reloads one conversation from a local JSON file.
- Show a warning when the estimated session cost crosses a user-configured budget.
- Add streaming output while preserving the same history and usage accounting behavior.

Stretch work does not replace required work and cannot raise a score above 100%.

## 9. Suggested stack

- Python 3.11+
- OpenAI or Anthropic Python SDK, called directly
- `python-dotenv` or environment variables for configuration
- `pytest` and `unittest.mock`

**Cheapest path:** Use a low-cost text model, short comparison prompts, a small output-token cap, and mocked automated tests. The program runs locally and needs no database, GPU, framework, or hosting service.

## 10. Estimated API cost

Expected model spend: **$0.05–$0.40**. This assumes a few short development sessions and the six required comparison conversations on a low-cost text model. Keep prices configurable, cap response length, monitor the provider dashboard, and set a **$1 project alert**. The assignment must remain below approximately **$5**.

