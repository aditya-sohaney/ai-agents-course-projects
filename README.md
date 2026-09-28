# AI Agents Course Projects

Eight project briefs for a university course that begins with Python programmers who have no prior LLM or agent experience and ends with a portfolio-grade, evaluated agent application.

The sequence begins with the mechanics of an LLM conversation, then treats an agent as a system you can inspect: a model chooses an action, tools change or retrieve information, observations return to the model, and tests determine whether the complete loop works. Projects 1–4 use raw OpenAI or Anthropic SDK calls. From Project 5 onward, you may use a framework such as LangGraph, but every assignment remains framework-agnostic.

## Progression

| # | Title | Core concept | Difficulty (1–5) | Est. hours | Solo/Group | Builds on |
|---:|---|---|---:|---:|---|---|
| 1 | [Hello, LLM](projects/01-hello-llm.md) | API calls, message roles, conversation history, tokens/cost | 1 | 3–5 | Solo | Python basics |
| 2 | [Your First AI Agent](projects/02-your-first-ai-agent.md) | Agent loop and one calculator tool | 1 | 4–6 | Solo | Project 1 |
| 3 | [Reliable Tool-Use Agent](projects/03-tool-use-agent.md) | Structured outputs, 1–3 tools, failure handling | 2 | 7–9 | Solo | Project 2 |
| 4 | [Stateful Knowledge Agent](projects/04-memory-and-rag-agent.md) | Turn state, memory, and local-document RAG | 3 | 12–16 | Solo | Projects 2–3 |
| 5 | [Measure What Matters: Agent Evals](projects/05-agent-evals.md) | Task success, regression tests, cost, and latency | 3 | 10–14 | Solo | Projects 3–4 |
| 6 | [Research Brief Studio](projects/06-multi-agent-system-%5BGROUP%5D.md) **[GROUP]** | Specialist agents, handoffs, and synthesis | 4 | 20–26 per team member | Group (3–4) | Projects 4–5 |
| 7 | [Human-in-the-Loop Operations Orchestrator](projects/07-orchestration-project-%5BGROUP%5D.md) **[GROUP]** | Planning, orchestration, approval gates, recovery | 5 | 24–32 per team member | Group (3–4) | Projects 5–6 |
| 8 | [Production Agent Capstone](projects/08-capstone.md) | Real-world product, deployment, evidence, reflection | 5 | 35–50 | Solo | Projects 1–7 |

## Course-wide policies

- **Language:** Python. A thin JavaScript or TypeScript front end is acceptable, but the agent logic must be implemented in Python.
- **Frameworks:** Projects 1–4 prohibit agent frameworks. Use direct OpenAI or Anthropic SDK calls so messages, loops, tool schemas, and state remain visible. Projects 5–8 may use a framework, but it is never required.
- **Compute:** Every project must run on a laptop with no GPU. Use hosted model APIs and local files or lightweight databases.
- **Infrastructure:** Do not pay for infrastructure. Free-tier hosting is acceptable when deployment is required.
- **Secrets:** Never commit API keys. Load secrets from environment variables and include a `.env.example` containing names only.
- **Costs:** Each project is designed to consume less than approximately $5 in model API credits when you use a low-cost text model, cache reusable outputs, cap iterations, and run the required evaluation set once before submission.
- **Responsible use:** Use synthetic, public, or explicitly authorized data. Document privacy, safety, and prompt-injection risks appropriate to your project.

## Submission conventions

Unless a project says otherwise, submit one repository URL and a release or commit hash representing the graded version. Your repository should run from a fresh clone by following its README. Include sample inputs, automated tests, and sanitized artifacts that let a grader verify behavior without access to private data.

Start every assignment from [`templates/project-spec-template.md`](templates/project-spec-template.md) when proposing a variation. Rubrics share the categories and performance language in [`templates/grading-rubric-template.md`](templates/grading-rubric-template.md). Instructor-facing calibration notes are in [`instructor-notes/grading-guidance.md`](instructor-notes/grading-guidance.md).
