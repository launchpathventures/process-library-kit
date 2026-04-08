# Process Library

A cumulative library of repeatable processes extracted from real work. Each process captures HOW a type of work gets done well, with a gold standard example and evaluation criteria for quality scoring.

## Quick Reference

- `/extract-process` — After completing work, analyze what was done and capture it as a reusable process
- `/run-process {name}` — Execute a named process with gold-standard evaluation
- `/list-processes` — Browse all available processes

## Where Processes Live

All processes are stored in `process-library/`. Each process is a directory:

```
process-library/{process-name}/
  process.md              — Definition: when to use, inputs, steps, outputs
  gold-standard.md        — Exemplar: a real example of excellent output (annotated)
  evaluation-criteria.md  — Rubric: 4-6 dimensions scored 1-5
```

## Process Quality Rule

A process is only as good as its gold standard. Every gold standard must be a REAL output from REAL work, not a hypothetical. If the work had flaws, note them — don't present flawed work as the standard without caveats.

## After Completing Work

When you finish a significant piece of work (building a feature, writing docs, conducting research, fixing a complex bug), ask: "This looks like a repeatable type of work. Would you like to run `/extract-process` to capture it in the process library?"

Only prompt when the work:
1. Will likely happen again (not a one-off fix)
2. Followed a discernible sequence of steps
3. Would benefit future sessions if captured

Do not prompt for trivial fixes or one-off tasks.
