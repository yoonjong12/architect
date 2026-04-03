---
name: review
description: >
  Structured protocol for producing review documents and evaluations — NOT code.
  Use when the user says "review", "evaluate", "리뷰", "검토", "평가", or asks for
  code review analysis, design critique, or assessment of an existing system.
  Enforces checkpoints and structured findings.
argument-hint: "[topic to review]"
allowed-tools: Read, Glob, Grep, Agent, AskUserQuestion
---

# Review Session

Produce a review document through a structured protocol. Output is evaluation and findings — no code unless explicitly requested.

## Workflow

1. **Read** `${CLAUDE_PLUGIN_ROOT}/references/session-protocol.md` — follow Session Protocol strictly
2. **Scope Lock** — establish what is being reviewed, boundary, exclusions, and output format
3. **Explore** — delegate codebase exploration to subagents, summarize findings
4. **Draft** — write the review document using the format below
5. **Checkpoint** — after each major section, present checkpoint and wait for confirmation
6. **Completion Gate** — verify all applicable checks before finalizing

## Review Document Format

```markdown
# [Topic] Review

## Scope
[What was reviewed]

## Method
[How the review was conducted]

## Findings
[Structured findings — tables preferred over prose]

## Recommendation
[Action items]
```

## Rules

- No code output unless the user explicitly requests it
- Prefer tables over prose for findings
- Match output format exactly to what was requested
- Each section must pass a checkpoint before proceeding
- Open items must be resolved before moving to the next section
