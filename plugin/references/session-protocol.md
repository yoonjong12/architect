# Session Protocol

Shared protocol for all architect skills. Every design or review session follows this structure.

## 1. Scope Lock

Before any exploration, establish scope with the user:

```
SCOPE LOCK:
- Topic: [what we're designing/reviewing]
- Boundary: [what is IN scope]
- Excluded: [what is NOT in scope]
- Output: [expected deliverable format]
```

Present this to the user and **wait for confirmation** before proceeding.

## 2. Explore and Summarize

Delegate codebase exploration to subagents. Present findings as a **concise summary**, not raw output.

| Do | Don't |
| --- | --- |
| Summarize findings in bullets | Dump raw code into conversation |
| Show only relevant excerpts | Read entire files into context |
| State what you found AND what it means | List files without interpretation |

## 3. Verify Before Design

When subagent research or exploration feeds into a design decision, **verify the findings before incorporating them**.

### Adversarial Self-Review

For each strategy or finding that enters the design, ask: **"What question from the user would break this?"**

- If you cannot answer, the research is incomplete — go back and investigate
- If the answer reveals a wrong assumption, fix it before presenting to the user
- This applies to subagent results, codebase exploration, and your own analysis

### No Vague Strategy

Words like "manual", "careful", "적절히", "as needed" are **not strategies**. They are TODOs disguised as decisions.

A strategy must specify **what** changes, **from where** the change comes, and **where** it goes:
- Bad: "Manual merge — combine both approaches"
- Good: "Take main's `_resolve_skills()` (returns `list[SkillRef]`), add our `submit_structured_feedback()` method after line 280. Remove our `_resolve_skill_contents()` — superseded by main's S3 presigned URL approach."

If a strategy cannot be stated concretely, it is an **Open item**, not a Decided item. It must not pass a checkpoint as Decided.

## 4. Checkpoint Gates

After each major section, output:

```
--- CHECKPOINT ---
Decided: [what was decided]
Open: [what's still unresolved]
Next: [what comes next]
```

Wait for user confirmation before proceeding. User may redirect, approve, or revise.

**Open items must be resolved before proceeding.** Open means "unresolved in this section", not "to be covered later". When Open items exist:
1. Present each Open item explicitly and ask the user for a decision
2. Do not proceed to Next until the user decides or explicitly approves deferral
3. If an Open item is a required input for the next section, it cannot be deferred — resolve it now

## 5. Output Rules

Return **exactly** what was requested:

- Single item asked → single item returned (no aggregation)
- Table asked → table only (no surrounding prose)
- Specific run/result → that exact data (no expansion)
- Design document → conceptual content (no code unless requested)

## 6. Completion Gate

Before declaring a document complete, verify all applicable checks:

```
COMPLETION GATE:
[ ] Delta Table: field-by-field comparison with borrowed system (N/A if none)
[ ] E2E Walkthrough: trace one concrete example through the entire pipeline (input → output, all stages)
[ ] Producer-Consumer: for each data consumer in the design, verify that a producer exists in the code
[ ] Impact Scope: flag whether undecided items affect only this module or propagate to other modules
```

- **Delta Table**: When borrowing from another system, verify the original mechanism from documentation (not memory) and specify per-field differences
- **E2E Walkthrough**: Trace with real values, not abstract schemas. Gaps surface here
- **Producer-Consumer**: When designing a consumer, always trace "who produces this data, when, and where" in the code. A field existing does not mean data flows. If no producer exists, state it as a precondition
- **Impact Scope**: If an undecided item affects only this module, it may be deferred. If it propagates to other modules, it must be resolved

## Anti-Patterns

| Pattern | Problem | Fix |
| --- | --- | --- |
| Producing code in a design session | Wrong output type | Ask: "Do you need code or a design document?" |
| Expanding scope mid-session | Scope drift | Re-read Scope Lock, ask user before expanding |
| Skipping checkpoint | User loses control | Always checkpoint after each section |
| Verbose report when table requested | Output mismatch | Match output format to user's request |
| Aggregating when specific asked | Data mismatch | Return exactly the item requested |
| "Borrowed from X" without Delta Table | Over/under-interpreting the original mechanism | Open the reference doc and write per-field comparison |
| Skipping E2E walkthrough | Individual roles look consistent but the pipeline has gaps | Trace one concrete example through input → output |
| Designing a consumer without a producer | A field exists but no data flows — produces an unimplementable design | Trace "who writes this value" in the code for every consumer |
| Proceeding with Open items | Unresolved decisions propagate as assumptions, causing rework later | Present Open items, get explicit decision or deferral approval before Next |
| Using subagent results without verification | Superficial strategies that collapse under one question | Apply Adversarial Self-Review (Section 3) before incorporating |
| "Manual merge" as a strategy | TODO disguised as decision — no concrete action specified | State exactly what changes, from where, and where it goes (Section 3) |
