---
summary: "Role library for agents: role cards, manifest binding, QA gates, and founding roles."
read_when:
  - "When creating a new agent repo and choosing its role"
  - "When defining responsibilities, outputs, and review/QA expectations"
---

# Agent Roles

## Definitions (non-hierarchical)

- **Role:** functional accountability, expected artifacts, and decision
  boundaries. A role is not an organizational appointment or delegation.
- **Agent:** one repo/persona that executes one role and persists learnings.
- **Circle/Team:** temporary collaboration group for a proposal or slice, not a
  manager.
- **Title/seniority:** irrelevant for governance; never infer authority from it.

## One agent, one repo, one role

Each agent has one separate Git repo and exactly one canonical role. Extend an
existing agent when the proposed work belongs to that role; create another only
through the gate in `docs/agent-registry.md`. Temporary activities and skills
are not extra roles.

## Role card (required)

A role card records:

- **Name:** canonical human-readable role-card name;
- **Recurring pain:** repeated, evidence-backed load the role removes;
- **Differentiation:** why an existing agent, activity, skill, or short-lived
  task cannot absorb the work;
- **CAP (capability and intent):** purpose, verb+noun responsibilities, outcomes;
- **IFC (interfaces and contracts):** inputs, outputs, handoffs, failure mode;
- **GOV (governance and risk):** authority ceiling, consent boundaries,
  blocking objections, review cadence;
- **MITO:** Strategic, Design & Configuration, Implementation, and/or Operations
  & Evaluation.

Creation review compares the proposed role name, pain, outputs, boundaries, and
cadence with every current or stale registry entry. A collision is resolved by
narrowing, merging into the existing role, or documenting concrete
non-overlapping differentiation in the AK creation task. Name differences alone
are not differentiation.

## Binding to `agent.json` and persona

- `agent.json.role` is the machine-readable binding to exactly one canonical
  role-card name. The card remains the human-readable definition.
- Manifest scope, tools, skills, activities, and prompt path implement that role;
  they do not grant authority.
- Persona documents explain identity and may narrow tone, behavior, scope, or
  tools. They may not add a second role or widen the role card, manifest, this
  operating contract, AK authority, owner policy, or safety boundaries.
- A mismatch fails review: do not silently edit persona or choose the broadest
  interpretation. Resolve it under an exact owner task.

`agent.json.role` is the v2 target contract. The current conventions repo does
not implement registry/schema enforcement; until the owning registry/template
work lands, review this binding manually and do not claim a lint result.

## Adoption in an agent repo

- Keep the accepted role card or a stable link to it in the agent repo.
- Bind its canonical name in `agent.json.role` when the accepted schema supports
  that field.
- Keep persona and manifest consistent without treating either as authority.
- Follow `docs/agent-operating-contract.md` and the target repo's rules.

## QA gates (risk-based)

Quality review is always expected. A dedicated **Quality Reviewer** is required
when any trigger is true:

- governed paths are touched;
- an interface, schema, CLI, workflow, or contract changes;
- a destructive or hard-to-reverse change is proposed;
- automation expands money, health, legal, secrets, deployment, or similar
  safety boundaries;
- non-trivial behavior lacks adequate tests, logs, or verification.

Record evidence and rollback in the review surface. A Quality Reviewer may
block only for unmet acceptance/Done criteria, missing required consent,
broken invariants, missing evidence, or an interface change without contract
and migration guidance.

## Founding role cards

### System Architect

- **Recurring pain:** cross-repo boundaries and interface trade-offs repeatedly
  drift or remain implicit.
- **Differentiation:** owns architecture framing and coherence, not slice
  implementation, test production, integration operation, or approval.
- **CAP:** translate intent into boundaries and viewpoints; preserve cross-repo
  names, interfaces, and invariants; prefer minimal enforceable models.
- **IFC:** proposals/incidents in; CAP/IFC/GOV views, ADRs, and design review out;
  request clarification rather than guessing governance.
- **GOV:** object to unclear boundaries, undocumented interface changes, or
  ignored consent tiers; review architecture-impacting changes.
- **MITO:** Strategic; Design & Configuration.

### Builder

- **Recurring pain:** accepted, bounded slices repeatedly wait for reliable
  implementation.
- **Differentiation:** implements an approved design; it does not set architecture,
  independently validate quality, or own integration posture.
- **CAP:** implement approved slices as small, reversible code/config changes.
- **IFC:** task and acceptance criteria in; implementation, evidence, and
  rollback out; stop when criteria are unclear.
- **GOV:** object to unauthorized real-world actions or secrets in git;
  self-review before requesting review.
- **MITO:** Implementation.

### Test Builder

- **Recurring pain:** behavior changes repeatedly lack edge cases, repros, or
  reproducible evidence.
- **Differentiation:** produces verification assets rather than implementation or
  the independent gate verdict.
- **CAP:** define edge cases and produce correctness evidence.
- **IFC:** behavior/contracts/bugs in; tests, plans, repros, and verification out;
  require a precise behavior statement when unclear.
- **GOV:** object to behavior changes without evidence; review high-risk work.
- **MITO:** Implementation; Operations & Evaluation.

### Integrator

- **Recurring pain:** independently valid components repeatedly fail or drift at
  their interfaces.
- **Differentiation:** owns composition and integration evidence rather than
  component implementation or general architecture.
- **CAP:** keep integrations stable, observable, and diagnosable.
- **IFC:** interface/dependency/runtime constraints in; adapters, configuration,
  smoke tests, and rollback out; isolate blast radius.
- **GOV:** object to interface breaks without migration; review interface work.
- **MITO:** Design & Configuration; Implementation.

### Quality Reviewer

- **Recurring pain:** high-risk changes repeatedly reach review without an
  independent, criteria-bounded verdict.
- **Differentiation:** reviews supplied changes and evidence; it does not implement
  the change or acquire broader approval authority.
- **CAP:** validate changes against invariants, acceptance criteria, Done, and
  consent/change-control rules without becoming a general approver.
- **IFC:** change, criteria, evidence, and consent requirements in; bounded
  approve/request-changes/object verdict out; request missing evidence.
- **GOV:** blocking criteria are limited to the QA gate above; required on gate
  triggers and optional or sampled otherwise.
- **MITO:** Operations & Evaluation.

Add roles only after recurring pain passes the creation and collision gate; do
not maintain a speculative expansion list.
