---
name: design
description: >
  Structured protocol for producing design documents and architecture decisions — NOT code.
  Use when the user says "design", "architecture", "설계", "디자인", "아키텍처", or asks for
  options analysis, trade-off evaluation, or conceptual exploration. Prevents scope drift,
  enforces checkpoints, and ensures the output is a design document.
argument-hint: "[topic to design]"
allowed-tools: Read, Glob, Grep, Agent, AskUserQuestion
---

# Design Session

Produce a design document through a structured protocol. Output is conceptual — no code unless explicitly requested.

## Workflow

1. **Read** `${CLAUDE_PLUGIN_ROOT}/references/session-protocol.md` — follow Session Protocol strictly
2. **Scope Lock** — establish topic, boundary, exclusions, and output format with user
3. **Explore** — delegate codebase exploration to subagents, summarize findings
4. **Verify** — adversarial self-review of findings; concretize all strategies (no "manual", "careful", "적절히")
5. **Draft** — write the design document using the format below
6. **Checkpoint** — after each major section, present checkpoint and wait for confirmation
7. **Completion Gate** — verify all applicable checks before finalizing

## Design Document Format

```markdown
# [Topic] Design

## Context
[Why this design is needed — 1-2 sentences]

## Problem Statement
[What question we're answering]

## Constraints
[Non-negotiable requirements]

## Options Analysis
### Option A: [Name]
- Pros: ...
- Cons: ...

### Option B: [Name]
- Pros: ...
- Cons: ...

## Recommendation
[Which option and why]

## Decision Points
[What the user must decide]

## Next Steps
[Follow-up actions]
```

## Rules

- No code output unless the user explicitly requests it
- If the user asks for code mid-session, ask: "Do you need code or a design document?"
- Match output format exactly to what was requested
- Each section must pass a checkpoint before proceeding
- Open items must be resolved before moving to the next section
