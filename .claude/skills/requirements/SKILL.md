---
name: requirements
description: Procedure for turning an ambiguous engineering request into a minimal, testable requirement specification.
---

# Requirements phase

Read `01-request.md` and inspect the repository relevant to the request.

Produce `02-requirements.md` with:

1. Problem and desired outcome.
2. Functional requirements.
3. Non-functional constraints that are actually relevant.
4. Scope boundaries.
5. Assumptions.
6. Acceptance criteria written so they can be verified.
7. Open questions, marking which are blocking.

Rules:

- Preserve user intent; do not expand scope for completeness alone.
- Reuse existing behavior as evidence.
- Distinguish facts from assumptions.
- If a question is not blocking, record a sensible assumption and continue.
- Do not prescribe implementation details unless they are an explicit constraint.
