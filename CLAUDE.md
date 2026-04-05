# Project Name

<!-- Run `init-agent` to configure this file -->

## Workflow

This project follows **Spec Driven Development (SDD)**. See agents in `.claude/agents/` for phase-specific instructions.

### Phases

0. **Init** — Run `init-agent` once to configure conventions and MCP servers
1. **Requirements** — `requirements-agent` → **critique-agent** loop → user approves
2. **Design** — `design-agent` → **critique-agent** loop → user approves
3. **Tasks** — `task-creator-agent` → **critique-agent** loop → user approves
4. **Implementation** — `implementation-agent` → delivers PRs → human reviews
5. **QA** — `qa-agent` → validates against specs
6. **Bug Fix** — `bug-fix-agent` → when issues are found

### Rules

- **Never skip phases.** Each phase produces a spec that the next phase consumes.
- **Review at phase gates.** Present output to the human for approval before moving on.
- **Keep specs anchored.** When implementation reveals spec gaps, update the spec first.
- **Specs live in `.specs/`** organized by feature. Templates in `.specs/_template/`.
- **Traceability via GitHub labels.** Each task in `tasks.md` lists traced IDs (FR, DD, NFR, EC). The `/create-issues` skill creates GitHub issues with corresponding labels (`req:FR-001`, `dd:DD-003`, etc.) for queryable traceability.

## Conventions

<!-- Populated by init-agent. Run it to configure. -->
