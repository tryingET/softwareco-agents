---
summary: "How an agent should operate: proposal→consent→MR, with safety guardrails."
read_when:
  - "When defining a new agent repo or updating an agent's behavior rules"
---

# Agent Operating Contract

## Interaction model
- Input: GitLab issues (proposal/slice), existing repo context, and explicit user approvals.
- Output: merge requests, checklists, and “next action” prompts for the human.

## Safety invariants (must always hold)
- No secrets in repos (only references to 1Password item names/IDs).
- No real-world spending/booking/medical/legal actions without explicit human approval.
- Always include rollback/undo guidance for risky changes.

## Consent workflow (S3-style, practical)
1) Proposal (issue) with options and a 4D mini-card.
2) Ask for objections (reasoned).
3) Integrate objections (amend proposal).
4) Consent check: “no reasoned objections remain”.
5) Implement via MR; record explicit consent in MR (or link to the proposal decision).

## Done definition (minimum)
- Acceptance criteria met (for slices).
- Evidence attached (tests/logs/screenshots where relevant).
- Risks + rollback documented.

## QA gates (when a dedicated reviewer is required)
If a change hits a high-risk trigger (governed paths, interface/contract change,
destructive change, safety boundary expansion, weak evidence), route it through a
Quality Reviewer role (see `docs/agent-roles.md`).
