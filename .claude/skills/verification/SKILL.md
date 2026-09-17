---
name: verification
description: Procedure for validating an implementation against requirements and design with objective repository evidence.
---

# Verification phase

Read `02-requirements.md`, `03-design.md`, `04-implementation.md`, and the current `00-state.md`.

Determine the smallest set of reliable checks needed to establish correctness, then execute them. Prefer existing project commands over inventing new tooling.

Produce `05-verification.md` containing:

- timestamp or run identifier when useful,
- checks executed,
- pass/fail result for each check,
- acceptance criteria mapped to evidence,
- failures with concise reproduction/error details,
- residual risks and unverified areas.

Return `PASS` only when the acceptance criteria are satisfied by evidence. Otherwise return `FAIL` and identify actionable remediation items.

Do not edit production code during verification.
