# Architect

A Claude Code plugin for structured design and review sessions.

Produces design documents and architecture reviews through a disciplined protocol — scope lock, checkpoint gates, and completion verification. No code output unless explicitly requested.

## Skills

| Skill | Command | Purpose |
|-------|---------|---------|
| Design | `/architect:design` | Create design documents with options analysis and recommendations |
| Review | `/architect:review` | Produce structured review documents with findings and action items |

## Session Protocol

Both skills follow a shared protocol:

1. **Scope Lock** — define topic, boundary, exclusions, and output format before starting
2. **Explore & Summarize** — investigate the codebase via subagents, present concise summaries
3. **Checkpoint Gates** — after each section, confirm decisions and resolve open items
4. **Completion Gate** — verify delta tables, E2E walkthroughs, producer-consumer chains, and impact scope

## Installation

```
/plugin marketplace add yoonjong12/architect
/plugin install architect@architect
```

## Usage

```
/architect:design authentication middleware
/architect:review current caching strategy
```

## License

MIT
