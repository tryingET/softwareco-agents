---
summary: "Additive manifest/role backfill for legacy agents without changing authored persona or useful operating flows."
read_when:
  - "When backfilling triage, health, consent-steward, or another pre-v2 agent"
---

# Legacy Agent Backfill

## Observed baseline (2026-08-27)

Read-only review found that `agent-triage`, `agent-health`, and
`agent-consent-steward` each have five authored persona inputs
(`identity`, `reason`, `main_task`, `dream_goal`, `behavior_rules`) plus a persona
README, four
System4D views, activity material, and GitLab proposal/MR templates. None has
`agent.json`. Their useful boundaries differ:

- triage keeps issues/MRs labeled, scoped, and routed;
- health plans and tracks routines and explicitly excludes medical action;
- consent-steward requires explicit human yes/no, does not infer silence, and
  excludes code, secrets, deployment, and protected-branch actions.

The common proposal template carries a 4D mini-card; MR templates retain scope,
evidence, risk, rollback, and consent links. These are review aids, not AK
runtime state.

## Additive-only procedure

Use one exact AK backfill task per agent repo.

1. Snapshot the clean worktree and inventory existing persona, System4D,
   activities, prompts, learnings, and MR/issue templates.
2. Run the creation-quality role, recurring-pain, differentiation, and collision
   review from `docs/agent-registry.md`. Backfill does not waive that gate.
3. Select an accepted role card whose scope is no broader than the existing
   persona. Stop on conflict; do not rewrite persona to make a card fit.
4. Add `agent.json` using the fleet schema accepted at execution time and bind
   its canonical role in `role`. Do not invent fields or claim registry support
   that has not landed.
5. Add only required generated or provenance artifacts explicitly authorized by
   that schema/task. Do not re-render the legacy repo.
6. Confirm the operating path still preserves useful 4D framing, explicit
   consent where required, review/evidence, and rollback while treating AK as
   runtime authority and GitLab as proposal/review.
7. Review a scoped diff, run the repo's validation, and attach evidence to the
   AK task. Lifecycle completion remains with the authorized owner.

## Preservation rule

Backfill must not edit or delete existing `docs/person/**`, `docs/system4d/**`,
`prompts/**`, `diary/**`, `docs/learnings/**`, or GitLab issue/MR templates.
If the accepted manifest cannot truthfully reference the existing identity
without such an edit, stop and open a separate owner decision/task. Never use a
Copier re-render as backfill.

## Verification checklist

- [ ] only additive, task-authorized paths changed;
- [ ] legacy persona bytes are unchanged;
- [ ] one repo, one agent, one canonical role;
- [ ] manifest values match rather than widen authored boundaries;
- [ ] role/pain/differentiation/collision review is linked;
- [ ] 4D, consent, MR/evidence, and rollback flows still work as review aids;
- [ ] no generated lint, runtime activation, consent, or lifecycle result is
      claimed unless the owning implementation actually produced it.
