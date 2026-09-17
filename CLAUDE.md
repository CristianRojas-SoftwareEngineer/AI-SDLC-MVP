# AI-SDLC project instructions

This repository uses the `AI-SDLC` workflow for changes that originate from a requirement and should pass through a complete development loop.

## Core rules

- Treat the repository's current implementation as the source of truth.
- Prefer the smallest coherent change that satisfies the requirement.
- Do not add abstractions, compatibility shims, frameworks, or infrastructure unless the requirement or existing architecture needs them.
- Keep the system in a canonical state: when a function, API, or design changes, update all affected call sites and remove obsolete code rather than maintaining parallel paths.
- Preserve existing project conventions unless the requirement explicitly changes them.
- Never claim a phase is complete without the artifact or evidence required by that phase.
- Do not silently invent business requirements. Record assumptions and unresolved questions.
- Do not deploy to an external or production target unless the user explicitly authorizes deployment.

## Language policy

- Interact with the user in Spanish: all conversational responses, questions, summaries, and outputs addressed to the user must be in Spanish.
- Write user-facing documentation in Spanish (`README.md`, `docs/`, `.ai-sdlc/`).
- Keep Claude Code artifacts in English for token efficiency: `CLAUDE.md`, `.claude/agents/`, `.claude/skills/`.
- Write source code in English for token efficiency (identifiers, comments, tests), unless the requirement explicitly states otherwise.

## AI-SDLC entrypoint

For the full autonomous workflow, start Claude Code with:

```powershell
claude --agent ai-sdlc
```

Then provide the requirement in natural language. You can also pass it directly after the command.

The technical Claude Code agent identifier is `ai-sdlc`; its role/display name is `AI-SDLC`.

## Project-level development expectations

- Inspect the repository before designing or editing.
- Reuse existing commands and test infrastructure whenever possible.
- Prefer deterministic, repository-local artifacts over long conversational state.
- Keep phase artifacts under `.ai-sdlc/tasks/<task-id>/`.
- A phase is complete only when its exit criteria are satisfied.
- Verification failures go back to implementation as a bounded remediation loop.
