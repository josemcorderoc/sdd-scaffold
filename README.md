# SDD Scaffold

A scaffold for AI-assisted web application development using **Spec Driven Development (SDD)** with [Claude Code](https://claude.com/claude-code).

This template provides the structure, agents, and workflow to take a project from product conception to production code — with human review at every gate.

## What's included

```
.claude/agents/
├── requirements-agent.md      # Phase 1: Ideas → structured requirements
├── critique-agent.md          # Adversarial review at any phase gate
├── design-agent.md            # Phase 2: Requirements → technical design
├── task-creator-agent.md      # Phase 3: Design → dependency-aware task breakdown
├── implementation-agent.md    # Phase 4: Tasks → production code via PRs
├── qa-agent.md                # Phase 5: Code → validation and review
└── bug-fix-agent.md           # Phase 6: Issues → fixes

.specs/
└── _template/
    ├── requirements.md        # Requirements document template
    ├── design.md              # Design document template
    └── tasks.md               # Task breakdown template

.github/
└── PULL_REQUEST_TEMPLATE.md   # Standardized PR format

CLAUDE.md                      # Steering document with workflow rules
```

## How it works

### The SDD Pipeline

```
Idea → Requirements → Critique → Design → Critique → Tasks → Critique → Implementation → QA → Ship
          ↑              ↓         ↑          ↓        ↑         ↓
          └── human approves ──────┴── human approves ──┴── human approves
```

Each phase produces a spec document. The **critique agent** challenges the output at every gate. The human approves before moving forward. Implementation happens only after specs are stable.

### Getting Started

1. **Use this template** to create your new project repo
2. **Edit `CLAUDE.md`** — replace "Project Name" with your project name and add project-specific conventions (tech stack, data sources, etc.)
3. **Add MCP servers** to `.mcp.json` for your stack (database connectors, Playwright, etc.)
4. **Start the workflow** — describe your idea to Claude Code, then run the `requirements-agent`

### The Workflow Loop

```
You: "I want to build [idea]"
Claude: runs requirements-agent → produces .specs/feature/requirements.md
Claude: runs critique-agent → challenges the requirements
Claude: revises → critique again → iterate until stable
You: approve requirements
Claude: runs design-agent → produces .specs/feature/design.md
... same critique loop ...
You: approve design
Claude: runs task-creator-agent → produces .specs/feature/tasks.md
... same critique loop ...
You: approve tasks
Claude: runs implementation-agent → delivers PRs for each task
You: review PRs → Claude addresses comments → merge
Claude: runs qa-agent → validates against specs
```

### Key Principles

- **Review at phase gates, not during implementation.** You review specs (a handful of documents), not hundreds of micro-decisions during coding.
- **Specs are a continuous feedback loop.** When implementation reveals gaps, update the spec first.
- **The critique agent is adversarial by design.** It finds the 3–5 strongest objections and proposes alternatives — don't manufacture objections if the spec is solid.
- **Evaluate review comments critically.** Not every suggestion warrants a code change — dismiss what adds complexity without benefit.

## Customization

### Adding project-specific conventions

Add to the `### Development Conventions` section in `CLAUDE.md`:

```markdown
- **Python tooling** — uv for dependency management, SQLAlchemy 2.x + Alembic for DB
- **Wikimedia compliance** — polite User-Agent, prefer dump files over API
```

### Adding MCP servers

Create `.mcp.json` in your project root:

```json
{
  "mcpServers": {
    "playwright": {
      "type": "stdio",
      "command": "npx",
      "args": ["-y", "@playwright/mcp@latest"]
    },
    "postgres": {
      "type": "stdio",
      "command": "npx",
      "args": ["-y", "@bytebase/dbhub", "--dsn", "postgresql://user:pass@localhost:5432/db"]
    }
  }
}
```

## License

MIT
