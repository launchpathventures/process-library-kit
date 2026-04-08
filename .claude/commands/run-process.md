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

## Step 2: Study the Reference Example

Before starting work, study the gold standard / reference example file. Focus on:
- **The example**: What does the critical output actually look like? What's the structure, format, level of detail?
- **The key decisions**: What tradeoffs were made and why? If you face similar decisions, follow the same reasoning unless the user's context demands otherwise.
- **The mistakes to avoid**: What specifically should you NOT do? These are your guardrails.

Do NOT just skim this. The key decisions section is the most valuable part — it captures the judgment that made the original output good.

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

## Step 6: Check Against Mistakes to Avoid

Do one final check against the "Mistakes to Avoid" section in the reference example. For each listed mistake:
- Did you avoid it? (yes/no)
- If no: where in your output does it appear?

This catches issues the checklist might miss — the checklist covers what SHOULD be there, the mistakes list covers what SHOULDN'T.

## Step 7: Save Run Record

Save a brief record to `process-library/{name}/runs/{YYYY-MM-DD}-{brief-description}.md`:

```
Date: {date}
Context: {what this was applied to}
Checklist: {N}/{total} passed (must-haves: {status})
Mistakes avoided: {N}/{total}
Key decisions: {any deviations from gold standard decision log, and why}
```

Create the `runs/` directory if it doesn't exist.

## Step 8: Feedback (CRITICAL — do not skip)

This is the most important step. It's how processes improve. One question to start, then follow the specific workflow for their answer.

### Ask one question:

"**How did that work?**
1. **Worked well** — the approach fit, maybe small tweaks
2. **Mostly worked, but went in a different direction** — some steps didn't apply or I had to go off-script
3. **Wrong approach** — this wasn't really the right process for this work"

Wait for their answer.

---

### Path A: "Worked well"

Ask ONE follow-up: "Anything we should tweak for next time?"

If the user mentions something, identify which type of improvement it is and follow the specific instructions:

**Missing step** → Open `process-library/{name}/process.md`. Add the new step in the correct position. Renumber subsequent steps. Read it back to confirm placement.

**Unnecessary step** → Open `process-library/{name}/process.md`. Remove the step. Add a note to "Common Pitfalls": "Step '{name}' was removed because {reason} — don't re-add it."

**Steps in wrong order** → Open `process-library/{name}/process.md`. Reorder and renumber. Read back the new order to confirm.

**New mistake to avoid** → Open `process-library/{name}/gold-standard.md`. Add a new entry to the "Mistakes to Avoid" section with: what it looks like, why it's a problem, what to do instead.

**Better example produced** → Ask: "Should this replace the current reference example?" If yes, open `process-library/{name}/gold-standard.md`. Replace the "Best Output Example" section with the relevant excerpt from this run's output. Update the Source and Date fields.

**New decision worth recording** → Open `process-library/{name}/gold-standard.md`. Add a new entry to "Key Decisions" with: what was chosen, alternatives, reasoning, when to reconsider.

**Checklist item was ambiguous** → Open `process-library/{name}/checklist.md`. Rewrite the item to be a clearer yes/no question. Read back the old and new versions to confirm.

**Missing checklist item** → Open `process-library/{name}/checklist.md`. Add the new item to the appropriate tier (Must Pass / Should Pass / Bonus).

If nothing to tweak: "Great — process is working well." Move on.

---

### Path B: "Mostly worked, but went in a different direction"

Ask: "Should we update this process to cover what happened, or save the new approach as its own process?"

**If they want to update this process:**

1. Ask: "What was different from the saved process? Was it the steps, the type of output, or both?"
2. Based on their answer, apply the relevant changes:
   - Open `process-library/{name}/process.md` — add, remove, or modify steps to cover the broader scope
   - Update the "When to Use" section to reflect the expanded scope
   - Open `process-library/{name}/gold-standard.md` — add new decisions that emerged, add new mistakes discovered
   - If the output from this run is more representative of the expanded process, ask if it should replace the example
   - Open `process-library/{name}/checklist.md` — add checks for the new scope
3. Read back all changes to the user for confirmation before saving
4. Update the slash command (`.claude/commands/{name}.md`) description if the scope changed significantly

**If they want a new process:**

1. Ask: "What would you call the new approach?" Suggest a name.
2. Run `/extract-process` — use the current session's work as the basis
3. After the new process is created, add each process to the other's "Related Processes" section:
   - In the original: "Related: `/{new-name}` — for when the work goes in {direction}"
   - In the new one: "Related: `/{original-name}` — for when the work stays closer to {original scope}"
4. Consider updating the original process's "When to Use" to clarify: "NOT for {what the new process covers}"

---

### Path C: "Wrong approach"

Ask: "Want me to save what we actually did as a new process, so you have the right one next time?"

**If yes:**
1. Run `/extract-process` for the work that was actually done
2. Update the original process's "When to Use" section to add: "NOT for {what this run was actually about}" — so this mismatch doesn't happen again
3. Note in the run record: "Process was a mismatch for this type of work. Created `/{new-name}` instead."

**If no:** Move on. Note in the run record: "Process was a mismatch but user chose not to extract a new one."

---

### After feedback: Record everything

Update the run record (`process-library/{name}/runs/{date}-{description}.md`) to include:
```
Improvements made:
- {list each change: "Added step 3a: ...", "Added mistake: ...", "Replaced example", etc.}
- {or "None — process worked as-is"}
```

Increment the run count in `process.md`. Update "Last updated" date if any files changed.

## Output

Present to the user:
1. Your complete output
2. The quality checklist with evidence
3. One-line summary: "{N}/{total} checks passed. {Must-have status}."

Then ask the feedback question.

After feedback, confirm: "Process `/{name}` updated: {specific list of what changed, e.g., 'added step 4a, added 1 mistake to avoid, updated example'}." or "No changes — process worked as-is."
