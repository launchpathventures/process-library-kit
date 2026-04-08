# Process Library

A cumulative library of repeatable processes extracted from real work. Each process captures HOW a type of work gets done well, with a gold standard example, decision log, anti-patterns, and a binary quality checklist.

## Quick Reference

- `/extract-process` — After completing work, analyze what was done and capture it as a reusable process with its own slash command
- `/list-processes` — Browse all available processes and their slash commands
- `/{process-name}` — Each process gets its own slash command for direct invocation

## Where Processes Live

All processes are stored in `process-library/`. Each process is a directory:

```
process-library/{process-name}/
  process.md              — Definition: when to use, inputs, steps, outputs
  gold-standard.md        — Exemplar excerpt + decision log + anti-patterns
  checklist.md            — Binary quality checklist (pass/fail, no partial credit)
  runs/                   — Automatic run records with scores
```

Each process also generates a slash command at `.claude/commands/{process-name}.md` for direct invocation.

## Gold Standard Design

A gold standard has three sections — each serves a different purpose:

1. **Exemplar** — A curated excerpt (the critical 20-30%, not the whole output). Real artifact, not hypothetical.
2. **Decision Log** — The key decisions and tradeoffs that shaped the output. Only genuine decisions where alternatives were considered — do not pad with obvious choices.
3. **Anti-Patterns** — Concrete things that would make the output BAD. Only realistic risks — do not pad with generic warnings.

## Quality Checklist Design

Quality is evaluated via a binary pass/fail checklist (not a numeric rubric). Each item:
- Is a concrete, verifiable yes/no question (a stranger could check it)
- Requires a one-line evidence citation (quote the output). No evidence = FAIL.
- Falls into must-have (all must pass), should-have (2/3 must pass), or nice-to-have (bonus)

Why checklist over rubric: numeric self-scoring is unreliable — Claude will rationalize a 4 on everything. Binary forces honesty: either the claim HAS a source cited, or it doesn't.

## Process Feedback Loop (critical)

Processes improve through structured feedback after every run. This is the most important part of the system. After every run, Claude must:

1. Save a run record automatically (checklist results, anti-pattern scan, key decisions)
2. Ask the user: **"How well did this process fit what you needed?"** with three options:
   - **Good fit** — proceed to capture specific improvements (missing steps, better examples, new anti-patterns, ambiguous checklist items)
   - **Partial fit** — determine whether to update this process or extract a new one for the divergent work
   - **Wrong process** — offer to extract what was actually done as a new process
3. Update process files immediately with any improvements identified
4. Record what changed in the run log

This three-way routing is essential. Without it, processes either never improve (no feedback) or accumulate scope until they're too broad to be useful (everything gets crammed into one process). The "partial fit" and "wrong process" paths ensure new processes get created when the work genuinely diverges.

## After Completing Work

When you finish a significant piece of work (building a feature, writing docs, conducting research, creating a proposal, fixing a complex bug), ask: "This looks like a repeatable type of work. Would you like to run `/extract-process` to capture it in the process library?"

Only prompt when the work:
1. Will likely happen again (not a one-off fix)
2. Followed a discernible sequence of steps
3. Would benefit future sessions if captured

Do not prompt for trivial fixes or one-off tasks.

## Process Extraction Quality

When extracting a process, always present the draft to the user for review BEFORE saving files. A bad process that gets saved and reused is worse than no process. The user must confirm:
- The steps match what actually happened
- The gold standard exemplar is the right section to highlight
- The checklist items can each be answered with a clear yes or no
