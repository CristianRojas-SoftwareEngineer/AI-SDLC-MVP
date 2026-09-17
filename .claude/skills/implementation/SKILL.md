---
name: implementation
description: Procedure for implementing an approved AI-SDLC design with narrow scope and structural coherence.
---

# Implementation phase

Read `02-requirements.md`, `03-design.md`, and the current `00-state.md`.

Before editing:

- inspect affected files,
- confirm repository commands,
- confirm the design still matches the codebase.

Then:

1. Implement the design.
2. Update all affected call sites and contracts.
3. Remove code made obsolete by the change unless compatibility is explicitly required.
4. Add or update focused tests.
5. Run the cheapest relevant checks first, then broader checks when warranted.
6. Update `04-implementation.md` with what changed, files touched, checks already run, and known limitations.

For remediation, start from the latest `05-verification.md` failure and address the root cause rather than masking the symptom.
