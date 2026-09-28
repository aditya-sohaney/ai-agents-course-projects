# Project 4 — Stateful Knowledge Agent

> **One-line pitch:** Build a raw-SDK assistant that remembers selected facts across turns and answers questions from a local document collection with verifiable citations.

## 1. Learning objectives

By the end of this project, you will be able to:

- Model short-term conversation state separately from durable user memory.
- Build a transparent retrieval-augmented generation (RAG) pipeline over local documents.
- Ground answers in retrieved evidence and decline when evidence is insufficient.
- Test session isolation, retrieval quality, citation validity, and memory behavior.

## 2. Prerequisites

- Completed Projects 2 and 3, including raw tool calls, schema validation, loop bounds, and failure observations.
- Familiarity with file I/O, SQLite or JSON persistence, and basic text processing.
- A model-provider API key. You do not need a GPU, hosted vector database, or paid document service.

## 3. Background and context

Conversation history is state, but it is not automatically useful memory. Sending an ever-growing transcript wastes tokens, preserves irrelevant details, and can mix information that should remain isolated. A stateful agent needs an explicit policy for what it keeps during a session, what it stores across sessions, how stored facts are updated, and when they are retrieved.

RAG gives a model access to a document set without training the model on those documents. A typical pipeline extracts text, creates chunks with stable source identifiers, retrieves a small set for a question, and asks the model to answer only from that evidence. Retrieval and generation are separate stages; logging them separately lets you locate failures instead of blaming the final response for everything.

Grounding is a behavioral contract, not a prompt slogan. Every factual claim covered by the collection should point to a retrieved chunk, and the system should say it lacks evidence when the collection does not support an answer. You will use simple local retrieval so the data flow remains visible, then evaluate both the retriever and the final answer.

## 4. The task

1. Build a Python CLI using direct OpenAI or Anthropic SDK calls. Agent and RAG frameworks are prohibited for this project.
2. Create or use an instructor-approved collection of at least six plain-text or Markdown documents and at least 6,000 total words. Use public, synthetic, or explicitly authorized content and record its provenance.
3. Implement deterministic ingestion: normalize text, split it into overlapping chunks, and assign every chunk a stable `document_id`, `chunk_id`, title, and source path. Re-running ingestion must not create duplicates.
4. Implement local retrieval without a paid service. Choose TF-IDF/BM25 or embeddings stored locally, return the top `k` chunks with scores, and expose retrieval through a validated `search_documents(query, k)` tool.
5. Maintain short-term session state for the current conversation and durable memory for a small allowlist of user preferences, such as preferred detail level or topic. Store durable facts in JSON or SQLite with `user_id`, provenance, and timestamp; provide explicit inspect and forget commands.
6. Never store a fact merely because it appeared in assistant output. Save a durable fact only when the user directly states it and the model submits a validated memory-write request. Isolate data between at least two test users.
7. Answer document questions with citations in the form `[document_id#chunk_id]`. Before display, verify that every cited ID was retrieved for that turn. If evidence is absent or conflicting, say so rather than filling the gap from model knowledge.
8. Keep a bounded context: at most the six most recent messages, retrieved chunks, and relevant durable memories. Cap the agent at five model calls and two retrieval calls per turn.
9. Log sanitized events for retrieval candidates and scores, memory reads/writes/deletes, model latency and usage, citations, and stop reason.
10. Build a labeled evaluation set with at least 15 cases: eight answerable document questions with expected source IDs, three unanswerable questions, two multi-turn memory cases, one memory-update case, and one cross-user isolation case.
11. Add automated unit tests for ingestion idempotence, retrieval, citation checking, memory update/deletion, user isolation, and both failure paths (missing index and malformed model arguments).

## 5. Deliverables and submission format

Submit:

- A GitHub repository URL and the commit hash to grade.
- Source, tests, dependency metadata, `.env.example`, document provenance, and a reproducible ingestion command.
- The local document set or a download script when redistribution is not allowed. Never submit private source material.
- The 15-case labeled dataset and a runner that reports retrieval hit rate, grounded-answer success, abstention success, and memory-case success.
- A README with an architecture diagram, state and memory policy, chunking/retrieval choices, setup, usage, privacy limitations, measured results, and actual API spend.
- A 1–2 minute demo showing a cited answer, a justified abstention, a preference recalled in a new session, and successful deletion.

## 6. Grading rubric

| Category | Weight | Evidence expected |
|---|---:|---|
| Functionality | 40% | Raw-SDK agent retrieves local evidence, verifies citations, abstains when needed, and implements isolated inspectable/deletable memory. |
| Code quality | 20% | Ingestion, retrieval, memory, orchestration, and verification have clear contracts; data and secrets are handled safely. |
| Evals/testing | 25% | Required unit tests pass and the 15-case report separates retrieval, answer, abstention, and memory performance. |
| Writeup/documentation | 15% | Architecture, provenance, memory policy, tradeoffs, limitations, results, setup, and spend are clear and accurate. |
| **Total** | **100%** | |

## 7. Estimated time and difficulty

- **Estimated time:** 12–16 hours
- **Difficulty:** 3/5
- **Format:** Solo

## 8. Stretch goals

- Compare lexical retrieval with local or API embeddings using the same labeled queries.
- Add a memory expiration policy and tests with a controllable clock.
- Detect likely prompt injection inside retrieved documents and exclude or clearly flag suspicious chunks.

Stretch work does not replace required work and cannot raise a score above 100%.

## 9. Suggested stack

- Python 3.11+
- OpenAI or Anthropic Python SDK, called directly
- SQLite or JSON for memory
- `scikit-learn` TF-IDF, `rank-bm25`, or provider embeddings stored in NumPy
- Pydantic or `jsonschema`, plus `pytest`

**Cheapest path:** Use TF-IDF or BM25 locally, Markdown documents, SQLite, a low-cost text model, cached model fixtures, and one final live evaluation run. No vector database, hosting, GPU, or paid document API is necessary.

## 10. Estimated API cost

Expected model spend: **$0.25–$1.50**. This assumes up to 100 bounded development calls and one 15-case evaluation run with short retrieved contexts on a low-cost model; lexical retrieval adds no API cost. Log usage per case, cache ingestion, cap chunk count and output length, and set a **$3 project alert**. The assignment must remain below approximately **$5**.
