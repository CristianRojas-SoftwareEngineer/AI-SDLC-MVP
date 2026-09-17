---
name: design
description: Procedure for producing a minimal repository-grounded technical design from approved requirements.
---

# Design phase

Read `02-requirements.md` and inspect the relevant repository structure and implementation patterns.

Produce `03-design.md` with:

1. Design goals and constraints.
2. Affected components/files.
3. Proposed control/data flow.
4. Interfaces, contracts, and state changes.
5. Detailed implementation steps in dependency order.
6. Verification strategy mapped to acceptance criteria.
7. Deployment/migration considerations only when applicable.
8. Key design decisions with rationale.

Rules:

- Prefer existing architectural patterns.
- Choose the smallest design that fully satisfies the requirements.
- Avoid speculative abstractions and framework changes.
- Identify breaking changes explicitly; do not preserve obsolete paths by default.
- Keep implementation details concrete enough that another agent can execute without rediscovering the design.
