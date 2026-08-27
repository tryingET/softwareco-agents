---
summary: "Deep design for the agent-fleet template system at scale: propagation mechanics, ownership split, interface contracts, lifecycle — decided via multi-lens analysis before L0 v2 lands."
read_when:
  - "Changing tpl-agent-repo at any layer (L0/L1/L2), the agent manifest convention, skill profiles, or the registry."
  - "Creating, updating, or retiring agents at fleet scale."
type: "design"
status: "ratified design; implementation proceeds by owning tasks"
---

# Agent fleet at scale — template system design (2026-08-27)

Trigger: operator call to review/re-propagate all agent documentation and
templates before the fleet grows ("it will get messy fast if we need to turn
1000s of agents around"). Scope: L0 `core/tpl-template-repo` (source of
truth), L1 renderings (softwareco, healthco), L2 agents (`ai-society/agents/
agent-*`), conventions (`softwareco-agents`), manifest v1, EC skill profiles,
registry (in flight).

## 0. Ground truth inventory (verified, not assumed)

- Fleet home: `ai-society/agents/agent-<name>`, one repo per agent
  (gitignored everywhere else BY DESIGN — no nesting). Existing: 3 legacy +
  1 steward (only v2-shaped).
- Lineage: L0 → L1 → L2 enforced by `layer-contract.yml` (reverse forbidden,
  no nesting, depth ≤ 2). Provenance sealed via `l0_source_sha` +
  render content hash in `.copier-answers.yml`.
- Drift measured: softwareco L1 is **100 L0 commits stale** (32 touching the
  agent template); healthco L1 diverges 19 files from softwareco L1; L0 has
  **no manifest**; conventions describe a GitLab-MR world without the AK
  runtime model.
- **Critical gap found:** the template has no exclusion/preservation
  mechanism — a copier re-render over an existing agent repo overwrites
  agent-owned content (persona, diary, learnings) wholesale.

## 1. First principles — what an agent repo actually is

An agent repo is simultaneously: **identity** (persona/system prompt),
**capability declaration** (manifest: skills/tools/thinking), **memory**
(diary → learnings → decisions), and **work templates** (activities).
At fleet scale only two of these want to be uniform: the *shape* of the
declaration (so machines can resolve agents) and the *authority model* (so
the fleet has uniform safety semantics). Identity content and memory are
necessarily per-agent. Therefore:

**Invariant I1 — ownership split.** Every file in an agent repo is either
TEMPLATE-OWNED (identical across the fleet, updated only by propagation) or
AGENT-OWNED (written only by the agent/its owner, never touched by
propagation). No third category. Ambiguity at 4 agents is an unsolvable
merge problem at 400.

Mapping: `AGENTS.md`, `CODEOWNERS`, `agent.json` schema shell,
`scripts/ci/*`, `contracts/*`, `governance/*` scaffolding, `docs/README`
skeletons = template-owned. `docs/person/**`, `docs/learnings/**`,
`docs/decisions/**`, `diary/**`, `prompts/activities/**`, `agent.json`
*values* (skills, tools, scope, version) = agent-owned.

**Invariant I2 — single identity source.** The five authored persona docs
(identity/reason/main_task/dream_goal/behavior_rules) are the authored soul;
the system prompt is **compiled from them**, not parallel to them. Today the
steward duplicates both and already drifted once. At 1000 agents, duplicated
identity is guaranteed incoherence. Template v2 ships a tiny compiler
(script in the repo, no deps) that renders `docs/person/system-prompt.md`
from the persona docs + manifest; the compiled file is generated, marked
`<!-- compiled: do not edit -->`, and CI checks freshness. The manifest's
`system_prompt_file` points at the compiled artifact.

## 2. Propagation mechanics — the actual "turn 1000 agents" lever

Three mechanisms, each with a defined role (choosing one and pretending the
others don't exist is how fleets rot):

1. **Birth** (new agent): copier render from L1 — unchanged, works.
2. **Template refresh** (existing agent, template changed): NOT a copier
   re-render. A propagation script (`propagate-template.sh`, shipped in the
   template itself) that: reads `.copier-answers.yml` provenance, renders
   the current template to a scratch dir, and **replaces only TEMPLATE-OWNED
   paths**, leaving agent-owned paths untouched; emits a diff for owner
   review; refuses to run if the ownership map is ambiguous (unknown file at
   top level = halt and classify). This is the mechanism that makes 1000
   agents turnable in one command each, mechanically.
3. **Interface refresh** (contract changed: manifest schema, EC skill
   profiles): registries fail closed today (unknown profile/skill = no
   spawn). Add a fleet-level `lint` (registry subcommand) that walks all
   `agent-*/agent.json`, checks schema version, profile/skill existence,
   compiled-prompt freshness, and provenance staleness — one command that
   answers "how broken is the fleet?" — plus per-agent one-line fix hints.

**Invariant I3 — propagation is pull-and-diff, never silent push.** No
mechanism ever writes into an agent repo without a visible diff and the
repo's owner applying it. (Consistent with the whole workspace's
owner-authority model.)

## 3. Interface contracts (the thing that actually breaks at scale)

**Invariant I4 — manifest schema is a versioned interface.**
`ai-society.agent/1` fields may be ADDED (consumers must ignore unknown
fields); REMOVED/RENAMED fields require a new schema integer and a registry
deprecation window. The registry supports N and N−1 concurrently.

**Invariant I5 — EC skill profiles are a published interface.**
`skills/profiles.json` gains `schema` + a stability rule: profile KEYS are
stable identifiers (rename = new key + old key kept as deprecated alias for
one EC release cycle); profile MEMBERSHIP may change freely (membership is
content, keys are API). Rationale: a rename today silently breaks every
agent referencing the profile — fail-closed at spawn, which at 1000 agents
means a fleet-wide outage class. EC CI gains a check: profiles referenced by
any registered agent.json may not be removed (registry lint output feeds it).

**Invariant I6 — authority model is uniform fleet-wide.** Template AGENTS.md
carries the authority section (parameterized): advisory-by-default; writes
to other repos go through exact AK tasks in the target repo; AK is runtime
authority, MRs are review surface; consent rules per the operating contract.
No agent invents its own authority semantics; persona may only NARROW.

## 4. Lifecycle (absent today — the quiet mess-maker)

- **Creation gate:** a new agent repo requires an AK task (in the fleet
  root's owning context) naming the role, the recurring pain it removes, and
  its differentiation from existing agents (registry lint checks role
  collisions). Cheap, prevents 1000-agent sprawl of near-duplicates.
- **Role binding:** one role per agent repo (existing conventions rule)
  recorded in `agent.json` (`role` field, free-text role-card name); registry
  indexes it.
- **Retirement:** an agent with no diary/learnings activity for a quarter
  shows as `stale` in registry lint; retirement = archive the repo + registry
  status row. Repos are cheap; zombies are not.

## 5. Goodhart guard

Fleet size is not a KPI. The registry reports **active** agents (receipts/
learnings in window), not total. The creation gate exists precisely so
"agents created" never becomes a number anyone optimizes.

## 6. Second-system guard (the G3 lesson applied to ourselves)

At 4 agents we design for 1000 by fixing INVARIANTS (ownership split,
compiled identity, interface versioning, pull-and-diff propagation,
fail-closed resolution) — not by building agent-government (no HR schemas,
no approval workflows beyond the AK task, no cross-agent org charts). Every
mechanism above is a script, a field, or a lint rule. Anything heavier waits
for demonstrated pain.

## 7. Execution plan (ordered; each step gated by the previous)

| # | Where | What | Gate |
|---|---|---|---|
| 1 | L0 `tpl-agent-repo` | v2: manifest shell + persona-compiler + ownership map + `propagate-template.sh` + authority section + fixture | L0 gate green; steward re-instantiation byte-check |
| 2 | conventions (`softwareco-agents`) | operating-contract v2 (AK+MR reconciliation), lifecycle (creation gate, retirement), roles↔manifest `role` field | review by independent session |
| 3 | EC | `profiles.json` schema + stability rule + CI check (I5) | suite green |
| 4 | L1 re-render | softwareco + healthco from new L0; reconcile 19-file sibling divergence | provenance seal updated; fixture diff reviewed |
| 5 | registry (5098) | add `role`, lint subcommand (I4/I5/freshness/staleness), collision check | tests incl. real fleet walk |
| 6 | L2 backfill | steward re-render via propagation script (dogfood); legacy three: additive manifests only | propagation diff reviewed by owner |

## 8. Ratified calls

a) **Persona docs:** keep the existing authored persona model; persona is
   agent-owned and may narrow, never widen, fleet authority.
b) **Creation gate strictness:** require one exact AK creation task per agent;
   registry checks supplement rather than replace this anti-sprawl gate.
c) **Legacy agents (triage/health/consent-steward):** backfill manifests
   additively and leave existing persona bytes untouched.
