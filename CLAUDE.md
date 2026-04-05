# Project Name

## Project

<!-- Fill in your project description, tech stack, and project-specific rules -->

### Tech Stack

- **Backend** — ...
- **Frontend** — ...
- **Database** — ...
- **Testing** — ...

### Project-Specific Rules

<!-- Add rules unique to your project (e.g., API compliance, data policies) -->

### MCP Servers

Configured in `.mcp.json`. Add servers relevant to your stack (e.g., Playwright for E2E testing, database connectors for queries during development).

## Workflow

This project follows **Spec Driven Development (SDD)**. See agents in `.claude/agents/` for phase-specific instructions.

### Phases

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

## Conventions

- **Clean Architecture** — Separate entities, use cases, interfaces, infrastructure. Dependencies point inward.
- **Test Driven Development** — Tests first. Red → Green → Refactor.
- **Conventional Commits** — `feat:`, `fix:`, `chore:`, `docs:`, `refactor:`, `test:`, `ci:`
- **Semantic Versioning** — `vMAJOR.MINOR.PATCH`
- **Feature branches + PRs** — No direct commits to `main`. All work via PRs with human review.
- **GitHub** — Use `gh` CLI for branches, PRs, and issues.
- **Latest versions** — Use the latest stable version of all dependencies. Pin in lock files.
- **SKILL.md** — Capture reusable patterns at the end of significant features.
