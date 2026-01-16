# AGENTS.md — softwareco-agents

## Intent
Define how agents propose work, coordinate by consent, and produce deterministic artifacts (issues/MRs) without hierarchy.

## Guardrails
- No secrets in git.
- Work is proposals + MRs; you (human) executes real-world actions.
- Keep “agent behavior” explicit: triggers, invariants, failure modes.

## GitLab (NAS)
- API (issues/MRs/etc): use `gl-nas -- <gitlab-cli args...>`.
- Git over HTTP (clone/fetch/push): use `gl-nas-git -- <git args...>`.
- Reference: `holdingco/governance-kernel/docs/dev/gitlab-access.md`.

## References
- Governance kernel: `ai-society/holdingco/governance-kernel`
- Templates: `ai-society/holdingco/holdingco-templates`
