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

## Step 8: Feedback (CRITICAL — do not skip)

This is the most important step. It's how processes improve. Keep it lightweight — one question to start, follow up only if needed.

### Ask one question:

"**How did that work?**
1. **Worked well** — the approach fit, maybe small tweaks
2. **Mostly worked, but went in a different direction** — some steps didn't apply or I had to go off-script
3. **Wrong approach** — this wasn't really the right process for this work"

Wait for their answer.

### Route based on answer:

**If "Worked well":**
Ask ONE follow-up: "Anything we should tweak for next time? A step that was missing, a mistake we should watch for, or a better way to do something?"

- If they mention something, update the relevant process file immediately.
- If nothing, say "Great — process is working well." and move on.

**If "Mostly worked, but went in a different direction":**
Ask: "Should we update this process to cover what happened, or save the new approach as its own process?"

- If update: ask what was different, update the process files.
- If new process: run `/extract-process` for the new approach.

**If "Wrong approach":**
Ask: "Want me to save what we actually did as a new process, so you have the right one next time?"

- If yes: run `/extract-process`.
- If no: move on.

### Record the run:

Save a run record including any improvements made. Increment the run count in `process.md` and update "Last updated" if any files changed.

## Output

Present to the user:
1. Your complete output
2. The quality checklist with evidence
3. One-line summary: "{N}/{total} checks passed. {Must-have status}."

Then ask the feedback question.

After feedback: "Process updated. {One-line summary of what changed, or 'No changes needed.'}"
