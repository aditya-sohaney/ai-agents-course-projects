# Project N — Title

> **One-line pitch:** Build ...

## 1. Learning objectives

By the end of this project, you will be able to:

- Explain the agent concept the project introduces.
- Implement and inspect the relevant agent behavior.
- Test the complete system rather than relying on an impressive demo.

## 2. Prerequisites

- Prior project(s): ...
- Concepts: ...
- Technical setup: Python, Git, and an LLM API key.

## 3. Background and context

Write two or three paragraphs that explain the new concept before assigning work. Connect it to behavior students have already implemented. Define unfamiliar terms and explain why the concept matters in a production system.

Use the final paragraph to name the central design tradeoff. Give students enough context to make a reasoned decision without prescribing one implementation.

## 4. The task

1. State a concrete functional requirement.
2. State the required agent behavior and constraints.
3. Define tool, state, or data contracts where applicable.
4. Require bounded failure handling and visible diagnostics.
5. Define a minimum automated test or evaluation set.
6. State security, privacy, and cost controls.

## 5. Deliverables and submission format

Submit:

- A repository URL plus the release tag or commit hash to grade.
- A README with setup, architecture, usage, limitations, and cost notes.
- Source code, tests/evals, `.env.example`, and sanitized sample data.
- A short demo and writeup in the format specified by the assignment.

## 6. Grading rubric

| Category | Weight | Evidence expected |
|---|---:|---|
| Functionality | 40% | Required behaviors work on the published acceptance cases. |
| Code quality | 20% | Clear modules, typed or documented interfaces, safe configuration, and reproducible setup. |
| Evals/testing | 25% | Meaningful automated cases, recorded results, and failure analysis. |
| Writeup/documentation | 15% | Accurate README, design rationale, limitations, cost, and demo evidence. |
| **Total** | **100%** | |

For a group project, use the group variant in `grading-rubric-template.md`, which reserves 10% for peer evaluation.

## 7. Estimated time and difficulty

- **Estimated time:** N–N hours
- **Difficulty:** N/5
- **Format:** Solo or group of 3–4

## 8. Stretch goals

- Optional extension one.
- Optional extension two.
- Optional extension three.

Stretch work does not replace required work and cannot raise a score above 100%.

## 9. Suggested stack

- Python 3.11+
- OpenAI or Anthropic Python SDK
- `pytest` and standard-library logging
- Optional libraries appropriate to the assignment

**Cheapest path:** Use a low-cost text model, local files or SQLite, deterministic fixtures, strict token and iteration caps, and free hosting if deployment is required.

## 10. Estimated API cost

Expected model spend: **$X–$Y**, with an absolute assignment budget below approximately **$5**. State the assumptions: model class, number of development calls, evaluation-set size, and output cap. Explain how students can monitor and limit spend.

<!--
Author checklist (remove before publishing the project):
- Title and one-line pitch
- Learning objectives
- Prerequisites
- Two or three background paragraphs
- Numbered, testable task requirements
- Deliverables and submission format
- Weighted rubric totaling 100%
- Estimated hours and difficulty 1–5
- Two or three stretch goals
- Suggested stack with cheapest path
- Estimated API cost below ~$5
-->

