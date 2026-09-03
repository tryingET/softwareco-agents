---
summary: "Candidate decomposition and retirement contract for the transitional softwareco-agents authority surface."
read_when:
  - "Moving agent roles, persona, history, or fleet conventions out of softwareco-agents."
  - "Reviewing whether this repository can be redirected or retired."
type: "contract-candidate"
status: "owner_acceptance_pending"
---

# softwareco-agents decomposition contract

## Decision status

`softwareco-agents` remains a transitional incubation, discovery, and migration-coordination repository. It is not the long-term semantic owner of society agents, company agents, governance law, provider content, release or permit state, runtime custody, or effects. This candidate contract does not retire, archive, delete, move, or activate anything. Retirement is a separately authorized Stage B action.

## Problem and evidence

The repository currently contains useful role catalog, operating-contract, registry, lifecycle, and historical fleet material. That material helped bootstrap the current fleet but combines conventions and transitional coordination that the ratified target assigns to different native owners. Leaving the repository as an indefinite authority surface would create duplicate ownership alongside governance-kernel, L0, appointing company parents, standalone child repositories, Prompt Vault/providers, Agent Kernel, Pi, registry, and ASC.

Immediate deletion would be equally wrong: current links, authored history, persona context, and migration evidence would be lost or made ambiguous. The minimum safe correction is an explicit decomposition contract with one target owner per concern, preserved history, durable redirects, compatibility checks, and a reversible retirement gate.

## Current and target roles

Current accepted role for this transition:

- inventory current and historical fleet material;
- coordinate owner handoffs and redirects;
- preserve source history and explain what is no longer authoritative;
- maintain compatibility until native owners accept replacements.

Target role after successful decomposition:

- historical and redirect surface only, or archived repository, as separately accepted;
- no live semantic ownership and no runtime or appointment claims.

## Ownership map

| Concern | Target semantic owner | This repository's residual role |
|---|---|---|
| Standing-agent authority and trust law | `tryingET/governance-kernel` | link to accepted law; retain historical discussion only |
| Generic `ai-society.agent/*` shape and provider-qualified references | `tryingET/core_tpl-template-repo` | link to accepted L0 contract |
| Society-appointed agent persona, policy, history, and local decisions | standalone repository under `~/ai-society/agents/agent-*` | preserve source/redirect evidence |
| Company-appointed agent persona, policy, history, and local decisions | standalone repository under `~/ai-society/<company>/agents/agent-*` | preserve source/redirect evidence |
| Company agents-lane baseline | appointing company parent | retain migration crosslink only |
| Provider profiles and governed content | provider owners and Prompt Vault | no authority through copying or import |
| Accepted release and task-bound permit concerns | Agent Kernel after its decision process | no shadow lifecycle ledger |
| Runtime loading and observation | Pi and registry owners | no launch or verification authority |
| Process/session/effect custody and disposition | ASC/effect owners | no effect settlement |

No concern may have two semantic owners after transfer, and no concern may be left without one.

## Decomposition invariants

1. **One agent, one standalone repository, one canonical role.** Agent-owned persona, policy, evidence, and source history move together or remain unchanged until they can.
2. **Home follows appointing jurisdiction.** Society and company homes are not selected from read territory, product affinity, provider, runtime host, or the current repository path.
3. **History is evidence.** Transfers preserve Git history or an exact digest-bound archive and crosslink; historical text is not rewritten to appear current.
4. **Redirect before retire.** Every current entrypoint must name the native owner artifact and classify retained material as current, historical, or forensic.
5. **Proxy transfer is not implementation completion.** A native issue, PR, or document must be explicitly accepted by its owner. Closing or copying a proxy is insufficient.
6. **No duplicate authority.** Once a native owner accepts a concern, this repository must cease making a competing normative claim.
7. **Retirement is reversible until final acceptance.** The repository remains available while any owner, link, compatibility, or rollback check is incomplete.

## Required sequence

1. Inventory every normative, descriptive, generated, and historical artifact and record its current revision.
2. Classify each item by concern and target semantic owner.
3. Resolve conflicts through the target owner's process; do not silently select convenient text.
4. Obtain native owner acceptance or explicit temporary acceptance of a named proxy artifact.
5. Publish redirects that include exact repository, path, revision, issue/PR, status, and known limitations.
6. Transfer or copy material without overwriting agent-owned bytes or severing source history.
7. Validate links, parent/child Git ownership, schema compatibility, and rollback.
8. Record an explicit Stage B retirement decision only after every replacement is accepted and the transitional surface has no remaining semantic owner obligation.

## Failure semantics

- `missing_target_owner`: decomposition is blocked; do not create a neutral owner merely for convenience.
- `owner_rejects_transfer`: retain the current transitional surface and produce the smallest corrective delta for that owner.
- `history_or_persona_loss_risk`: stop; no transfer or retirement is eligible.
- `dual_authority_after_transfer`: block retirement and remove the duplicate normative claim.
- `broken_redirect_or_rollback`: classify as not ready for retirement.
- `native_artifact_unaccepted`: the proxy remains non-authoritative even when complete.

These outcomes remain local to this transition map and do not become a global error enum.

## Compatibility, migration, and rollback

The contract is additive. Current repository contents and links remain available. No agent moves, schema default changes, or runtime paths change during Gate A.

Rollback means leaving `softwareco-agents` active as the transitional surface, reverting only unaccepted redirects, and restoring the prior routing map. Rollback must not delete transferred history or automatically revive duplicate semantic authority. A concern already accepted by its native owner remains native unless that owner explicitly reverses it.

## Acceptance scenarios

The transitional owner may accept this contract when:

- every concern has one named target semantic owner;
- society and company placements follow accountable appointment;
- agent-owned bytes and source history are recoverable after transfer;
- redirects distinguish current owner contracts from historical material;
- parent and child Git ownership do not overlap;
- native acceptance is recorded rather than inferred from issue or PR state;
- retirement remains a distinct Stage B decision with a tested rollback path.

## Alternatives rejected

- **Keep this repository as the universal fleet owner:** rejected because it duplicates semantic owners and conflicts with appointing jurisdiction.
- **Delete it after copying files:** rejected because it loses history, breaks links, and can hide unresolved owner choices.
- **Treat a proxy issue as transfer:** rejected because issue existence or closure is not semantic acceptance.
- **Create a neutral shared repository for every cross-owner concern:** rejected because the integration fixtures fit existing L0 and pi-extensions surfaces without adding a universal owner.
- **Use one monorepo for all agents:** rejected because child identity, history, appointment, and lifecycle require standalone ownership.

## Reversal triggers

Reopen the selected decomposition if a target owner cannot preserve required history or compatibility, if any transfer creates two semantic owners or none, if an accepted architecture decision assigns a continuing native concern here, or if rollback cannot restore unambiguous routing. Convenience, document length, or a desire to close Gate A are not reversal triggers.
