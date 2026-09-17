---
name: sdlc-orchestration
description: Defines the canonical AI-SDLC workflow, artifact contracts, phase gates, remediation loop, and stop conditions for end-to-end development from a vague requirement.
argument-hint: [requirement]
---

# Canonical AI-SDLC workflow

Use this workflow whenever the current session is acting as `AI-SDLC`.

## Lifecycle

1. **Requirements** — discover what should change and why.
2. **Design** — decide the smallest coherent technical change.
3. **Implementation** — modify the repository.
4. **Verification** — gather objective evidence that the implementation satisfies requirements.
5. **Delivery** — summarize the change, evidence, residual risks, and release readiness.
6. **Maintenance** — treat new defects, feedback, or change requests as input to another Requirements phase.

The loop is intentionally asymmetric: not every SDLC phase deserves a dedicated agent. Agents are selected by cognitive specialization and context isolation, not by one-to-one phase counting.

## Task workspace

Create:

`.ai-sdlc/tasks/<task-id>/`

with:

- `01-request.md` — original request plus normalized intent.
- `00-state.md` — canonical workflow state.
- `02-requirements.md` — requirements and acceptance criteria.
- `03-design.md` — technical design and implementation plan.
- `04-implementation.md` — implementation summary and touched areas.
- `05-verification.md` — verification evidence and failures.
- `06-delivery.md` — final delivery record and residual risks.

Use a short filesystem-safe task id. Do not create additional artifacts unless they materially improve traceability.

## Phase gate rules

### Requirements gate

Must contain:

- problem statement
- desired outcome
- in-scope behavior
- explicit out-of-scope behavior when useful
- constraints
- assumptions
- acceptance criteria
- unresolved questions

If an unresolved question blocks implementation, status is `BLOCKED`.

### Design gate

Must contain:

- affected components/files
- proposed change
- data/control flow where relevant
- key interfaces or contracts
- test strategy
- migration/deployment implications when applicable
- rejected alternatives only when they clarify a real decision

The design must be implementable without guessing the architecture.

### Implementation gate

Must contain:

- implementation completed against the design
- relevant call sites updated
- obsolete paths removed when superseded
- focused tests/checks added or updated
- implementation artifact updated

### Verification gate

Must report:

- checks executed
- results
- requirement-to-evidence mapping
- failures or residual risks

`PASS` advances to Delivery. `FAIL` returns to Implementation.

### Delivery gate

A task is `DONE` when:

- verification passed,
- implementation is integrated in the working tree,
- no blocking question remains,
- residual risks are explicitly recorded,
- deployment has been performed only if explicitly authorized.

## Remediation loop

On `verification = FAIL`:

1. Update `00-state.md` with the failure and increment `remediation_attempt`.
2. Delegate to `implementation-engineer` with `02-requirements.md`, `03-design.md`, and the relevant `05-verification.md` findings.
3. Re-run `verification-engineer`.
4. Stop after 2 remediation attempts for the same task if verification still fails.

Do not endlessly iterate.

## Human gates

Do not interrupt for routine implementation choices.

Ask the user only when:

- a business requirement is genuinely ambiguous and affects acceptance behavior, or
- a high-impact architectural decision has multiple materially different valid outcomes, or
- deployment to an external target requires explicit authorization.

When asking, present the exact missing decision and the concrete information needed to proceed.

## Compact orchestration rule

Pass artifacts, not transcript history. A specialist should receive the task id and the paths to its predecessor artifacts. The artifact is the durable contract; the returned handoff is only a summary.
