# Run Process

You are executing a named process from the process library and evaluating output against its gold standard.

NOTE: If this process has a dedicated slash command (check `.claude/commands/` for a file matching the process name), tell the user they can invoke it directly next time (e.g., `/{process-name}`).

## Arguments

`$ARGUMENTS` — first word is the process name, remaining words are context about what to apply it to.

Example: `/run-process competitive-research for CRM tools in the SMB market`

## Step 1: Load the Process

Read these files in order:
1. `process-library/{process-name}/gold-standard.md` — study this FIRST
2. `process-library/{process-name}/process.md` — the steps
3. `process-library/{process-name}/checklist.md` — the quality checklist

If the process doesn't exist, tell the user and run `/list-processes`.

## Step 2: Internalize the Gold Standard

Before starting work, study the gold standard. Focus on:
- **The exemplar**: What does the critical output actually look like? What's the structure, format, level of detail?
- **The decision log**: What tradeoffs were made and why? If you face similar decisions, follow the same reasoning unless the user's context demands otherwise.
- **The anti-patterns**: What specifically should you NOT do? These are your guardrails.

Do NOT just skim this. The decision log is the most valuable part — it captures the judgment that made the original output good.

## Step 3: Execute the Process Steps

Follow `process.md` step by step:
1. Read each step's description
2. Gather the specified inputs
3. Do the work
4. Produce the specified output

At each decision point, check the gold standard's decision log. If you're facing a similar choice, follow the documented reasoning. If the situation is genuinely different, note why you're deviating.

If a step requires human judgment, pause and ask.

## Step 4: Run the Quality Checklist

After producing the complete output, go through every item in `checklist.md`:

```
## Quality Checklist

### Must-Have
- [ ] PASS / FAIL — {checklist item} — Evidence: "{quote from your output}"
...

### Should-Have
- [ ] PASS / FAIL — {checklist item} — Evidence: "{quote}"
...

### Nice-to-Have
- [ ] PASS / FAIL — {checklist item} — Evidence: "{quote}"
...

**Result:** {N}/{total} passed | Must-haves: {all passed / X failed} | Should-haves: {N}/{M} passed
```

Rules:
- Every item gets a one-line evidence citation — quote the specific part of your output that proves pass/fail
- No evidence = FAIL (if you can't point to proof, it didn't happen)
- Be honest. A checklist only works if you don't rationalize borderline items as passes.

## Step 5: Revise if Needed

**Any must-have failed:** Stop. Revise the specific failing sections. Re-check ONLY the items that failed (don't re-run the whole checklist). Present the revision with updated checklist.

**Less than 2/3 should-haves passed:** Flag to the user: "Output is below standard on {N} should-have items. Want me to revise, or is this acceptable for your use case?" Let the user decide — not every run needs perfection.

## Step 6: Anti-Pattern Scan

Do one final check against the gold standard's anti-patterns list. For each anti-pattern:
- Did you avoid it? (yes/no)
- If no: where in your output does it appear?

This catches issues the checklist might miss — the checklist covers what SHOULD be there, anti-patterns cover what SHOULDN'T.

## Step 7: Save Run Record

Save a brief record to `process-library/{name}/runs/{YYYY-MM-DD}-{brief-description}.md`:

```
Date: {date}
Context: {what this was applied to}
Checklist: {N}/{total} passed (must-haves: {status})
Anti-patterns avoided: {N}/{total}
Key decisions: {any deviations from gold standard decision log, and why}
```

Create the `runs/` directory if it doesn't exist.

## Step 8: Improve the Process

After presenting results, ask the user:

"Did this run reveal anything worth capturing? For example:
- A step that was missing or unnecessary?
- A new anti-pattern we hit?
- A better example for the gold standard?
- A checklist item that was ambiguous or should be added?"

If the user identifies improvements, update the relevant process files now. This is how processes get better over time.

## Output

Present to the user:
1. The complete process output
2. The quality checklist with evidence
3. Anti-pattern scan results
4. One-line summary: "{N}/{total} checks passed. {Must-have status}. {Key decision or deviation if any.}"

Tip: "Next time, run this directly with `/{process-name}`."
