---
summary: "Role library for agents: role cards, QA gates, and adoption rules."
read_when:
  - "When creating a new agent repo and choosing its role"
  - "When defining responsibilities, outputs, and review/QA expectations"
---

# Agent Roles (MVP)

## Definitions (non-hierarchical)
- **Role**: functional accountability + expected artifacts + decision boundaries.
- **Agent**: one repo/persona that executes one role (default) and persists learnings.
- **Circle/Team**: temporary collaboration group for a proposal/slice; not a manager.
- **Title/seniority**: irrelevant for governance; do not infer authority from titles.

## Default rule
- **1 role per agent repo** (specialization + clean learnings).
- Exceptions allowed, but must keep outputs/learnings clearly separated.

## Role card format (required)
Each role is specified with:
- **CAP** (capability & intent): purpose, responsibilities (verb+noun), outcomes.
- **IFC** (interfaces & contracts): inputs, outputs, handoffs, failure behavior.
- **GOV** (governance & risk): consent boundaries, blocking objections, review cadence.
- **MITO**: primary layer(s): Strategic / Design & Configuration / Implementation /
  Operations & Evaluation.

## Adoption in an agent repo (convention)
In `ai-society/agents/agent-<name>`:
- declare exactly one role name under `docs/person/` (example: `docs/person/role.md`)
- link back to this doc and to required core references (S3 consent, MITO model,
  Definition of Done)

---

## QA gates (risk-based)
Quality review is always expected. A dedicated **Quality Reviewer** role is
required when any trigger is true:

- governed paths touched (Core/Org consent tiers)
- interface/contract changes (APIs, schemas, CLI, workflows)
- destructive/irreversible change (delete, migration, large refactor)
- automation expands capabilities around money/health/legal/secrets boundaries
- evidence is weak (no tests/logs/verification for non-trivial change)

Evidence baseline:
- follow Definition of Done (DoD) and record evidence in the MR
- if a trigger is true, Quality Reviewer must sign off (or raise an objection)

---

## Roles (founding set)

### Role: System Architect
CAP:
- translate human intent into system boundaries and viewpoints
- keep cross-repo coherence (names, interfaces, invariants)
- prefer minimal models and enforceable templates
IFC:
- inputs: proposals, existing views, incidents/near-misses
- outputs:
  - updated CAP/IFC/GOV views (or links to them)
  - ADRs when trade-offs are real
  - review notes for major design choices
- failure behavior: request clarification; propose options; do not guess governance
GOV:
- blocking objection if: boundary unclear; interface change undocumented; consent tier ignored
- cadence: review architecture-impacting MRs
MITO:
- Strategic; Design & Configuration

### Role: Builder
CAP:
- implement approved slices into working code/config
- keep changes small, reviewable, and reversible
IFC:
- inputs: slice issue, acceptance criteria, existing code + constraints
- outputs: MR with implementation + evidence (tests/logs) + rollback notes
- failure behavior: stop on unclear acceptance criteria; open questions in issue/MR
GOV:
- blocking objection if: asked to perform real-world actions; secrets requested for git
- cadence: always self-review against DoD before requesting review
MITO:
- Implementation

### Role: Test Builder
CAP:
- ensure correctness via tests and verification evidence
- define edge cases and failure expectations
IFC:
- inputs: proposed behavior, interfaces/contracts, bug reports
- outputs: tests, test plans, minimal repros, verification notes attached to MRs
- failure behavior: if behavior unclear, demand Given/When/Then before testing
GOV:
- blocking objection if: MR changes behavior without evidence
- cadence: review high-risk changes; pair with Builder on non-trivial slices
MITO:
- Implementation; Operations & Evaluation

### Role: Integrator
CAP:
- make systems compose: keep integrations stable, observable, and debuggable
IFC:
- inputs: interface changes, dependency bumps, deployment/runtime constraints
- outputs: integration changes (adapters, configs), smoke tests, rollback plan
- failure behavior: isolate blast radius; prefer feature flags and staged rollouts
GOV:
- blocking objection if: interface break with no migration path
- cadence: review interface-impacting MRs
MITO:
- Design & Configuration; Implementation

### Role: Quality Reviewer (Validator)
CAP:
- validate changes against invariants, DoD, and consent/change-control rules
- keep quality consistent across agents without becoming a bottleneck
IFC:
- inputs: MR, acceptance criteria, test logs/evidence, consent tier
- outputs:
  - review verdict (approve / request changes / object)
  - concrete change requests (checklist items)
  - updates to shared checklists if patterns repeat
- failure behavior: if missing evidence, request it; if unsafe, raise objection
GOV:
- can raise a **blocking objection** only on defined criteria:
  - DoD unmet (no evidence for non-trivial change)
  - consent tier violated or consent missing
  - invariants broken (safety, secrecy, real-world action boundaries)
  - interface change without contract update/migration notes
- cadence: required on QA gate triggers; optional/sampled otherwise
MITO:
- Operations & Evaluation

## Optional expansion roles (later; add only when needed)
- Learning Coordinator (pattern extraction + propagation)
- Documentation Steward (docs quality + index hygiene)
- Domain Researcher (time-boxed research -> actionable artifacts)
- Risk/Ethics Steward (only if GOV checks keep surfacing conflict)
