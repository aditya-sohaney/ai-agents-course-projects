# Grading Rubric Template

Use the solo rubric unless the assignment is explicitly marked `[GROUP]`. Score the published requirements, not framework choice or visual polish beyond what the assignment requests.

## Solo project rubric

| Category | Weight | Excellent | Competent | Developing | Incomplete |
|---|---:|---|---|---|---|
| Functionality | 40% | All acceptance criteria work; failures are bounded and understandable. | Core path works; minor edge cases fail. | Core path is inconsistent or partly manual. | Required system does not run. |
| Code quality | 20% | Cohesive modules, explicit contracts, safe secrets, reproducible setup. | Generally clear; some coupling or setup friction. | Difficult to change or reproduce. | Unsafe, missing, or largely nonfunctional code. |
| Evals/testing | 25% | Representative automated cases, reproducible results, useful failure analysis. | Basic tests cover the main path and results are reported. | Few shallow tests or unrepeatable claims. | No meaningful automated evidence. |
| Writeup/documentation | 15% | Clear architecture, decisions, limitations, costs, and usable instructions. | Required documentation is present with small gaps. | Important setup or reasoning is missing. | Documentation cannot support grading. |
| **Total** | **100%** | | | | |

## Group project rubric

| Category | Weight | Excellent | Competent | Developing | Incomplete |
|---|---:|---|---|---|---|
| Functionality | 35% | End-to-end workflow and integrations satisfy all acceptance criteria. | Core workflow works; minor edge cases fail. | Modules work separately but integration is unreliable. | Required system does not run. |
| Code quality | 15% | Shared contracts, ownership boundaries, safe configuration, reproducible setup. | Mostly clear with limited integration debt. | Coupled modules or unclear interfaces slow the team. | Unsafe or unmaintainable implementation. |
| Evals/testing | 25% | Unit, contract, and end-to-end evidence reveal meaningful system behavior. | Main path and key contracts are tested. | Tests are narrow or results are unrepeatable. | No meaningful automated evidence. |
| Writeup/documentation | 15% | Architecture, team decisions, operations, limitations, costs, and demo are clear. | Required documentation is present with small gaps. | Important setup or reasoning is missing. | Documentation cannot support grading. |
| Peer evaluation | 10% | Specific, corroborated evidence of ownership, collaboration, review, and integration. | Contributions match the agreed role and teammates' evidence. | Contribution or collaboration was uneven. | Little corroborated contribution or peer report missing. |
| **Total** | **100%** | | | | |

## Scoring procedure

1. Run the documented setup from a fresh clone or clean virtual environment.
2. Run automated tests and the submitted evaluation command before reviewing the demo.
3. Score each category against the assignment-specific evidence table.
4. Record one concrete strength, one observed failure, and one next step.
5. For group work, score the first four categories at team level and use peer evidence to adjust only the peer-evaluation category unless academic-integrity policy requires escalation.

