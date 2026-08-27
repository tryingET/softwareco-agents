---
summary: "Fleet discovery and lifecycle contract: one repo per agent, AK creation gate, staleness, and retirement."
read_when:
  - "When creating, finding, backfilling, reviewing, or retiring an agent"
---

# Agent Registry and Lifecycle

## Boundary

Agents live as separate Git repos at:

- `ai-society/agents/agent-<name>`

One repo represents one agent; do not nest agents or place several agents in a
monorepo. This document is a discovery/convention surface. AK remains runtime
authority for creation tasks, decisions, delegation, evidence, and lifecycle.
A registry row or repository's existence grants no authority.

## Creation gate (anti-sprawl)

Before creating a repo, obtain an exact AK creation task in the fleet-owning
context. The task must include:

1. the proposed canonical role-card name and role;
2. observed recurring pain, with concrete examples or receipts;
3. expected inputs, outputs, boundaries, and review cadence;
4. differentiation from current and stale agents;
5. collision results against registry rows, existing `agent-*` repos, manifests,
   role cards, and known activities/skills;
6. why extending an existing agent or using a finite task is insufficient;
7. the owner and lifecycle review date.

Unresolved collision, missing recurring evidence, or novelty based only on a
name blocks creation. Fleet size is not a KPI.

After the task is accepted, create exactly one repo, bind one canonical role in
`agent.json.role` when supported, and retain the AK task link as provenance.
Repository creation, role assignment, organizational delegation, and runtime
activation remain separate decisions.

## Lifecycle

Lifecycle review is owner-dispositioned, not inferred from automation:

- **current:** owner has current evidence that the recurring role is useful;
- **stale candidate:** no qualifying diary/learning/receipt activity for 90 days,
  or the role, authority, owner, manifest, or review date is unclear;
- **retired:** the accountable owner accepted retirement in AK.

A stale candidate does not execute new recurring work until the owner confirms,
narrows, merges, or retires it. Retirement archives the repo, disables runtime
activation, and retains a registry row with the decision/replacement link; it
does not erase persona, diary, learnings, decisions, or evidence.

No collision or staleness automation is implemented in this conventions repo.
Apply these checks manually until the owning registry tooling lands, and label
results as manual observations rather than lint output.

## Fleet inventory observed 2026-08-27

This is a filesystem/document review, not a claim of active runtime status:

| Agent | Authored role/scope observed | Repo | Manifest posture observed |
| --- | --- | --- | --- |
| `agent-triage` | issue/MR triage; labeling, routing, hygiene | `ai-society/agents/agent-triage` | no `agent.json` |
| `agent-health` | health planning/tracking; no medical actions | `ai-society/agents/agent-health` | no `agent.json` |
| `agent-consent-steward` | explicit, linked consent hygiene; no code execution | `ai-society/agents/agent-consent-steward` | no `agent.json` |
| `agent-adoption-steward` | advisory engineering-core adoption stewardship | `ai-society/agents/agent-adoption-steward` | `ai-society.agent/1`; no `role` field |

The three legacy repos retain authored persona, System4D, activity, and GitLab
proposal/MR material. Backfill is additive; see
`docs/legacy-agent-backfill.md`.
