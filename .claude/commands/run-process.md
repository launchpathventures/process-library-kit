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

## Step 8: Process Feedback (CRITICAL — do not skip)

This is the most important step. After presenting the output and evaluation, walk the user through a structured feedback conversation. This is where the learning happens.

### 8a: Assess fit

First, determine whether this process was the right one for the work. Ask the user:

"**How well did this process fit what you needed?**
1. **Good fit** — the steps matched the work, just needs minor tweaks
2. **Partial fit** — some steps were relevant but the work diverged significantly
3. **Wrong process** — this was actually a different type of work"

Wait for their answer before proceeding.

### 8b: Route based on fit

**If "Good fit"** — proceed to improvement questions (Step 8c).

**If "Partial fit"** — ask: "It sounds like this started as `/{process-name}` but became something different. Two options:
- **Update this process** to cover the broader scope (if the work is a natural evolution)
- **Create a new process** for the different part (if it's genuinely a separate type of work)
Which feels right?"

If they want a new process, run `/extract-process` for the divergent work. If they want to update, proceed to Step 8c with a note to expand scope.

**If "Wrong process"** — ask: "No problem — should we extract what you actually did as a new process? That way next time you'll have the right one." If yes, run `/extract-process`.

### 8c: Capture specific improvements

Only ask this if the process was a good or partial fit. Go through each area:

"Let me check a few things to make this process better for next time:"

1. **Steps**: "Were there any steps that were **missing** (something you had to figure out that should be documented)? Or steps that were **unnecessary** (wasted time, didn't apply)?"

2. **Gold standard**: "Was the example still relevant? Did this run produce something **better** that should replace it?"

3. **Decisions**: "Did you face any **new tradeoffs** that aren't in the decision log? Or did the existing decisions lead you astray?"

4. **Anti-patterns**: "Did you almost make a mistake or hit an issue that should be **warned against** for next time?"

5. **Checklist**: "Were any checklist items **ambiguous** (hard to answer yes/no)? Or is there a quality check that's **missing**?"

For each item the user identifies, update the relevant process file immediately. Don't batch — update as you go so nothing gets lost.

### 8d: Record what changed

Add a summary of improvements to the run record:

```
Improvements made:
- {what was changed and why}
```

Increment the run count in `process.md` and update the "Last updated" date if any process files were changed.

## Output

Present to the user:
1. Your complete output
2. The quality checklist with evidence
3. Anti-pattern scan results
4. One-line summary: "{N}/{total} checks passed. {Must-have status}. {Key decision or deviation if any.}"

Then run the feedback conversation (Step 8).

After feedback is complete, confirm: "Process `/{process-name}` updated with your feedback. {Summary of what changed, or 'No changes needed.'}. Run record saved."
