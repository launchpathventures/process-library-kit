# Process Library

A cumulative library of repeatable processes extracted from real work. Each process captures HOW a type of work gets done well, with a gold standard example, decision log, anti-patterns, and a binary quality checklist.

## Quick Reference

- `/extract-process` — After completing work, analyze what was done and capture it as a reusable process with its own slash command
- `/list-processes` — Browse all available processes and their slash commands
- `/{process-name}` — Each process gets its own slash command for direct invocation
- `/update-process-library` — Check for and install framework updates

## Update Check

When any process library command is run (`/extract-process`, `/run-process`, `/list-processes`, or any auto-generated process command), do a quick version check in the background:

1. Read `process-library/VERSION` for the local version
2. Run: `curl -s https://raw.githubusercontent.com/launchpathventures/process-library-kit/main/process-library/VERSION`
3. If the remote version is higher, add a one-line note at the end of your response: "A Process Library update is available (v{local} → v{remote}). Run `/update-process-library` to update."
4. If the check fails (network issue) or versions match, say nothing.

This check must NEVER block or delay the main command. Do the work first, mention the update at the end if applicable.

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

## Reference Example Design

Each process has a reference example with three sections:

1. **Best Output Example** — The critical 20-30% of the output, not the whole thing. A real artifact, not hypothetical.
2. **Key Decisions** — The decisions and tradeoffs that shaped the output. Only genuine decisions where alternatives were considered — do not pad with obvious choices.
3. **Mistakes to Avoid** — Concrete things that would make the output BAD. Only realistic risks — do not pad with generic warnings.

## Quality Checklist Design

Quality is evaluated via a binary pass/fail checklist (not a numeric rubric). Each item:
- Is a concrete, verifiable yes/no question (a stranger could check it)
- Requires a one-line evidence citation (quote the output). No evidence = FAIL.
- Falls into must-have (all must pass), should-have (2/3 must pass), or nice-to-have (bonus)

Why checklist over scores: numeric self-scoring is unreliable. Binary forces honesty: either the claim HAS a source cited, or it doesn't.

## Process Feedback Loop (critical)

Processes improve through structured feedback after every run. This is the most important part of the system. After every run, Claude must:

1. Save a run record automatically (checklist results, anti-pattern scan, key decisions)
2. Ask the user: **"How did that work?"** with three options:
   - **Worked well** — ask one follow-up: "Anything to tweak for next time?" Update if yes.
   - **Mostly worked, but went in a different direction** — ask whether to update this process or save the new approach as its own
   - **Wrong approach** — offer to save what was actually done as a new process
3. Update process files immediately with any improvements identified
4. Record what changed in the run log

This three-way routing is essential. Without it, processes either never improve (no feedback) or accumulate scope until they're too broad to be useful (everything gets crammed into one process). The "partial fit" and "wrong process" paths ensure new processes get created when the work genuinely diverges.

## After Completing Work

When you finish a significant piece of work (building a feature, writing a document, conducting research, creating a proposal, fixing a complex problem), ask: "Want me to save how we did that so we can use the same approach next time?"

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
