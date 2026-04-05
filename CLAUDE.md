# Project Name

## Spec Driven Development Workflow

This project follows a Spec Driven Development (SDD) workflow. All feature work flows through structured phases with human review at each gate — not during implementation.

### SDD Maturity Levels

We aim for **spec-anchored** development (level 2 of 3):

1. **Spec-first** — A spec is written upfront, then used to guide development (minimum bar)
2. **Spec-anchored** — Specs are kept alive after implementation, updated as the feature evolves (our target)
3. **Spec-as-source** — The spec is the primary artifact; humans edit only the spec, never the code

Specs are not a linear pipeline — they are a continuous feedback loop. At every phase, revisit earlier specs and update them when reality diverges from the plan.

### Workflow Phases

1. **Requirements** — Run `requirements-agent` to produce the spec
   - **Critique** — Run `critique-agent` on the output. Iterate: the requirements agent addresses objections, the critique agent re-reviews, until objections are resolved or the user decides.
   - **User approves** requirements before moving on.
2. **Design** — Run `design-agent` to produce the technical design
   - **Critique** — Same loop: critique agent challenges, design agent revises, iterate until stable.
   - **User approves** design before moving on.
3. **Tasks** — Run `task-creator-agent` to produce the task breakdown
   - **Critique** — Same loop.
   - **User approves** tasks before moving on.
4. **Implementation** — Run `implementation-agent` to write production code following the approved specs
5. **QA** — Run `qa-agent` to validate quality with tests and structured review
6. **Bug Fix** — Run `bug-fix-agent` when issues are found during QA or after merge

### Spec Directory Structure

All specs live in `.specs/` organized by issue or feature. For larger efforts, use sub-projects with phases:

```
.specs/
├── _template/
│   ├── requirements.md
│   ├── design.md
│   └── tasks.md
├── feature-name/
│   ├── requirements.md
│   ├── design.md
│   ├── tasks.md
│   └── review.md
├── large-feature/              # Sub-project organization
│   ├── requirements.md         # Top-level requirements for the whole effort
│   ├── phase-1-name/
│   │   ├── design.md
│   │   └── tasks.md
│   ├── phase-2-name/
│   │   ├── design.md
│   │   └── tasks.md
│   └── review.md
```

### Rules

- **Never skip phases.** Each phase produces a document that the next phase consumes.
- **Review at phase gates.** Present output to the human for approval before moving to the next phase.
- **Keep specs anchored.** When implementation reveals spec gaps, update the spec, don't just fix the code. Regularly revisit specs throughout the project.
- **Present options, not open-ended questions.** When clarifying requirements, offer numbered selectable choices to reduce iteration. Leave the last option for open user input.
- **Break work into testable chunks.** Each task should be independently verifiable. Build stepwise — small, testable modules before integrating.
- **Integrate learnings.** After completing a feature, update CLAUDE.md with new conventions and consider creating a SKILL.md entry for reusable patterns discovered during the build.

### Development Conventions

- **Clean Architecture** — Separate concerns into layers (entities, use cases, interfaces, infrastructure). Dependencies point inward. Framework and DB choices are infrastructure details, not core logic.
- **Test Driven Development** — Write tests first, then implement. Red → Green → Refactor. Every PR must include tests that cover the new functionality. Use pytest for Python, Vitest for frontend, Playwright for E2E.
- **Conventional Commits** — All commit messages must follow the [Conventional Commits](https://www.conventionalcommits.org/) spec: `feat:`, `fix:`, `chore:`, `docs:`, `refactor:`, `test:`, `ci:`, etc.
- **Semantic Versioning** — Follow [semver](https://semver.org/). Tag releases as `vMAJOR.MINOR.PATCH`.
- **Feature branches + PRs** — All work happens on feature branches. No direct commits to `main`. Every feature/fix is merged via a Pull Request with review. See **Pull Request Workflow** below.
- **GitHub repo** — The project is hosted on GitHub. Use `gh` CLI for creating branches, PRs, and managing issues.
- **Latest versions** — Use the latest stable version of all dependencies. Pin versions in lock files.

### Pull Request Workflow

Every task is delivered as a PR. The PR goes through human code review before merging.

#### Creating a PR

- Branch naming: `feat/<task-id>-<short-description>` (e.g., `feat/t1a-init-repo-docker`)
- PR body must include `Closes #<issue-number>` to auto-close the linked GitHub issue on merge
- Use the PR template in `.github/PULL_REQUEST_TEMPLATE.md`

#### Handling Review Comments

When the reviewer leaves comments on a PR:

1. **Fix the code** — address each comment with a commit
2. **Reply to every comment** — explain what was done (reference the commit) or why it couldn't be addressed
3. **Never resolve threads** — Claude always replies but never marks threads as resolved. Thread resolution is the human reviewer's responsibility

Never leave review comments without a reply. The reviewer should see a clear resolution for each point they raised.

All PR comments made by Claude must end with a signature line: `— 🤖 Claude`

#### Evaluating Review Comments

Not every review comment warrants a code change. Before acting on any comment (human or automated), assess critically:

- **Is it a real bug?** Fix it.
- **Does it add unnecessary complexity?** Dismiss it — simple is better, especially for local dev.
- **Is it relevant to the current scope?** If it belongs to a future task, dismiss with a note.
- **Is it a style preference with no functional impact?** Dismiss unless it aligns with project conventions.

Accept sound comments with a fix and a commit reference. Dismiss unsound ones with a clear, respectful explanation. Never blindly apply suggestions that add complexity without clear benefit.

#### After Merge

- The linked GitHub issue auto-closes via `Closes #N`
- Update the task checklist in `.specs/<feature>/tasks.md` to mark the task as done

### MCP Servers

The project has MCP servers configured in `.mcp.json`. Add MCP servers relevant to your project (e.g., Playwright for E2E testing, database connectors for direct queries during development).

### SKILL.md

At the conclusion of significant features or projects, capture reusable patterns and capabilities in `SKILL.md`. This accelerates future projects by codifying what was learned.
