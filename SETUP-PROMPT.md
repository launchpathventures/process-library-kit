# Setup Prompt

Copy everything below the line and paste it into your Co-Work (or Claude Code) session.

---

I want you to set up a Process Library system in this project. This gives us the ability to extract repeatable processes from work we do together, save them with gold standard examples, and replay them later with quality evaluation. Each process gets its own slash command.

Please create all of the following files exactly as specified.

## File 1: CLAUDE.md (append to existing, or create new)

If a `CLAUDE.md` already exists, append this section to the end. If not, create the file with this content:

```
# Process Library

A cumulative library of repeatable processes extracted from real work. Each process captures HOW a type of work gets done well, with a gold standard example, decision log, anti-patterns, and a binary quality checklist.

## Quick Reference

- `/extract-process` — After completing work, analyze what was done and capture it as a reusable process with its own slash command
- `/list-processes` — Browse all available processes and their slash commands
- `/{process-name}` — Each process gets its own slash command for direct invocation

## Where Processes Live

All processes are stored in `process-library/`. Each process is a directory:

process-library/{process-name}/
  process.md              — Definition: when to use, inputs, steps, outputs
  gold-standard.md        — Exemplar excerpt + decision log + anti-patterns
  checklist.md            — Binary quality checklist (pass/fail, no partial credit)

Each process also generates a slash command at `.claude/commands/{process-name}.md` for direct invocation.

## Gold Standard Design

A gold standard has three sections — each serves a different purpose:

1. **Exemplar** — A curated excerpt (the critical 20-30%, not the whole output). Real artifact, not hypothetical.
2. **Decision Log** — The 3-6 key decisions and tradeoffs that shaped the output. This is what actually prevents future runs from making worse choices.
3. **Anti-Patterns** — 3-5 concrete things that would make the output BAD. More useful than praise.

## Quality Checklist Design

Quality is evaluated via a binary pass/fail checklist (not a numeric rubric). Each item:
- Is a concrete, verifiable yes/no question
- Requires a one-line evidence citation (quote the output). No evidence = FAIL.
- Falls into must-have (all must pass), should-have (2/3 must pass), or nice-to-have (bonus)

Why checklist over rubric: numeric self-scoring is unreliable — Claude will rationalize a 4 on everything. Binary forces honesty: either input IS validated before processing, or it isn't.

## After Completing Work

When you finish a significant piece of work (building a feature, writing docs, conducting research, fixing a complex bug), ask: "This looks like a repeatable type of work. Would you like to run `/extract-process` to capture it in the process library?"

Only prompt when the work:
1. Will likely happen again (not a one-off fix)
2. Followed a discernible sequence of steps
3. Would benefit future sessions if captured

Do not prompt for trivial fixes or one-off tasks.
```

## File 2: `.claude/commands/extract-process.md`

```
# Extract Process

You are analyzing recently completed work to extract a repeatable process for the process library.

## Gather Context

Before anything, collect evidence of what just happened. Use whichever sources are available:

1. **Check if this is a git repo**: Run `git status`. If it works, also run `git diff HEAD~3` and `git log --oneline -10` to see what changed and in what order. If git is not available, skip this — the other sources are sufficient.
2. **Read key files**: Inspect the most important files that were created or modified during this session.
3. **Conversation context**: Review what the user asked for and the steps you took together. This is often the richest source of information about the process.

## Step 1: Identify the Process

Ask the user:
- What type of work was this? (e.g., "writing an API endpoint", "competitive research", "bug triage")
- Would you do this type of work again?
- What would you name it?

Propose a kebab-case name: `api-endpoint-scaffold`, `competitive-research`, `bug-triage-and-fix`.

## Step 2: Extract the Steps

From the git diff and conversation, reconstruct what happened:

1. What was researched/read first? (inputs)
2. What decisions were made? (judgment points)
3. What was produced? (outputs)
4. What was the sequence? (steps)

Generalize into reusable steps. Remove project-specific details. Keep the structure.

For each step document:
- **What**: The action taken
- **Why**: What this step accomplishes
- **Inputs**: What information is needed
- **Output**: What this step produces
- **Judgment calls**: Where human decisions are needed (or "none")

## Step 3: Build the Gold Standard

The gold standard is NOT the entire output dumped into a file. It is a curated reference that makes future runs better. Build it in three sections:

### Section A: The Exemplar (curated excerpt)

Pick the **most representative 20-30% of the output** — the part that best demonstrates the pattern. This is the critical excerpt, not the whole thing.

- If the output was code: the most important file or function, not every file touched
- If the output was a document: the best section, not the full doc
- If the output was research: the key findings and the structure, not every detail

Include the excerpt in full, exactly as produced (real artifact, not hypothetical).

### Section B: Decision Log

Document the **key decisions and tradeoffs** that shaped the output. This is what actually makes a gold standard useful — not "this is good" annotations, but "we chose X over Y because Z."

For each decision:
- **Decision**: What was chosen
- **Alternatives considered**: What else could have been done
- **Why this choice**: The reasoning
- **What would change it**: Under what conditions you'd choose differently

3-6 decisions is typical. These are what prevent future runs from making worse choices.

### Section C: Anti-Patterns

List 3-5 concrete things that would make this output BAD. These are more useful than vague praise because they give Claude specific things to avoid.

Format:
- **Anti-pattern**: {what bad looks like}
- **Why it's bad**: {the consequence}
- **What to do instead**: {the fix}

## Step 4: Build the Quality Checklist

Instead of a numeric rubric (which Claude will game by giving itself 4s), create a **binary checklist** — each item is PASS or FAIL with no middle ground.

Create 8-12 checklist items organized into three tiers:

### Must-Have (all must pass, or output needs revision)
Items where failure means the output is not usable. 4-5 items.
- Each item is a concrete, verifiable yes/no question
- Example: "Does every API endpoint validate input before processing?"

### Should-Have (most should pass)
Items where failure means the output is below standard but still usable. 3-4 items.
- Example: "Are error messages specific enough to diagnose without checking logs?"

### Nice-to-Have (bonus quality)
Items that distinguish good from great. 2-3 items.
- Example: "Does the test suite include at least one edge case per validation rule?"

**Passing criteria:**
- All must-haves pass
- At least 2/3 of should-haves pass

**Why a checklist, not a 1-5 rubric:** Numeric self-scoring is unreliable — Claude will rationalize a 4 on everything. Binary pass/fail forces honest assessment: either the input IS validated before processing, or it isn't. No wiggle room.

## Step 5: Write the Files

Create the process directory and files:

1. `process-library/{name}/process.md` — use `process-library/_template.md`
2. `process-library/{name}/gold-standard.md` — three sections from Step 3
3. `process-library/{name}/checklist.md` — the quality checklist from Step 4

Update `process-library/README.md` to include the new process in the index table.

## Step 6: Generate the Slash Command

This is critical — create a dedicated slash command so the user can invoke this process directly without remembering the name.

Create `.claude/commands/{process-name}.md` with this content:

# {Process Title}

You are executing the **{process-name}** process. This is a repeatable process extracted from real work.

## Context from user

$ARGUMENTS

## Load the process

Read these files in order:
1. `process-library/{process-name}/gold-standard.md` — study this FIRST. Internalize the decisions, the anti-patterns, and the exemplar quality level.
2. `process-library/{process-name}/process.md` — the steps to follow.
3. `process-library/{process-name}/checklist.md` — the quality checklist you will evaluate against when done.

## Execute

Follow the steps in `process.md`. At each step, refer back to the gold standard's decision log — if you face a similar decision, follow the same reasoning unless the user's context demands otherwise.

## Evaluate

After completing all steps, run through every item in `checklist.md`:
- For each item, state PASS or FAIL with a one-line evidence citation (quote the specific part of your output)
- If any must-have fails: stop and revise before presenting to the user
- Present the checklist results alongside your output

## Output

Present:
1. Your complete output
2. The checklist evaluation (with evidence per item)
3. A one-line summary: "{N}/{total} checks passed. {Must-have status}. {Any notable decisions made.}"

## Step 7: Verify

Read all files back and check:
- Could someone unfamiliar follow this process?
- Is the gold standard exemplar genuinely representative (not the whole output, just the critical part)?
- Does the decision log capture real tradeoffs (not just "we did the obvious thing")?
- Are anti-patterns concrete enough to recognize?
- Are checklist items binary and verifiable (not subjective)?
- Does the slash command work as a standalone invocation?

## Output

Tell the user:

Process extracted: `{name}`

Files created:
- `process-library/{name}/process.md` — {N} steps
- `process-library/{name}/gold-standard.md` — exemplar + {N} decisions + {N} anti-patterns
- `process-library/{name}/checklist.md` — {N} checks ({N} must-have, {N} should-have, {N} nice-to-have)
- `.claude/commands/{name}.md` — invoke directly with `/{name}`

To run this process: `/{name} [context about what to apply it to]`
To browse all processes: `/list-processes`
```

## File 3: `.claude/commands/run-process.md`

```
# Run Process

You are executing a named process from the process library and evaluating output against its gold standard.

NOTE: If this process has a dedicated slash command (check `.claude/commands/` for a file matching the process name), tell the user they can invoke it directly next time (e.g., `/{process-name}`).

## Arguments

`$ARGUMENTS` — first word is the process name, remaining words are context about what to apply it to.

Example: `/run-process api-endpoint-scaffold for the user notifications endpoint`

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

### Must-Have
- [ ] PASS / FAIL — {checklist item} — Evidence: "{quote from your output}"

### Should-Have
- [ ] PASS / FAIL — {checklist item} — Evidence: "{quote}"

### Nice-to-Have
- [ ] PASS / FAIL — {checklist item} — Evidence: "{quote}"

**Result:** {N}/{total} passed | Must-haves: {all passed / X failed} | Should-haves: {N}/{M} passed

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

## Step 7: Save Run Record (Optional)

If the user wants to track runs, save to `process-library/{name}/runs/{YYYY-MM-DD}-{brief-description}.md`:

Date: {date}
Context: {what this was applied to}
Checklist: {N}/{total} passed (must-haves: {status})
Anti-patterns avoided: {N}/{total}
Key decisions: {any deviations from gold standard decision log, and why}

## Output

Present to the user:
1. The complete process output
2. The quality checklist with evidence
3. Anti-pattern scan results
4. One-line summary: "{N}/{total} checks passed. {Must-have status}. {Key decision or deviation if any.}"

Tip: "Next time, run this directly with `/{process-name}`."
```

## File 4: `.claude/commands/list-processes.md`

```
# List Processes

Browse the process library and show available slash commands.

## Process

1. Read `process-library/README.md` for the index
2. If README is stale, scan `process-library/*/process.md` to build the list dynamically
3. Check `.claude/commands/` for matching slash commands per process
4. For each process, extract from `process.md`:
   - **Name**: Process name
   - **Slash command**: The direct invocation (e.g., `/{name}`)
   - **Description**: One-line summary (from "When to Use")
   - **Steps**: Number of steps
   - **Checklist items**: Number of quality checks
   - **Last used**: Most recent file in `runs/`, or "never"

Present as:

## Process Library

| Command | Description | Steps | Checks | Last Used |
|---------|-------------|-------|--------|-----------|
| `/{name}` | {desc} | {n} | {n} | {date} |

If the library is empty:

"No processes yet. After completing work, run `/extract-process` to capture it."

After listing:
- "Run any process directly: `/{name} [context]`"
- "Or use the generic runner: `/run-process {name} [context]`"
- "Extract a new process: `/extract-process`"
```

## File 5: `process-library/README.md`

```
# Process Library

Repeatable processes extracted from real work. Each includes steps, a gold standard (exemplar + decisions + anti-patterns), and a binary quality checklist.

## Commands

Every process gets its own slash command. Use `/list-processes` to see them all, or:

| Command | What it does |
|---------|-------------|
| `/{process-name}` | Run a specific process directly |
| `/extract-process` | Capture a new process from recently completed work |
| `/list-processes` | Browse all available processes |
| `/run-process {name}` | Generic runner (prefer the direct command) |

## Index

| Command | Description | Steps | Checks | Created |
|---------|-------------|-------|--------|---------|
| *(none yet — run `/extract-process` after completing work)* | | | | |

## How Processes Improve

1. **Gold standards get replaced** when a better exemplar emerges
2. **Decision logs grow** as new tradeoffs are encountered
3. **Anti-patterns accumulate** from real failures during runs
4. **Checklist items sharpen** when ambiguous items get rewritten
5. **Steps refine** when runs reveal missing or unnecessary steps
```

## File 6: `process-library/_template.md`

```
# Process: {Process Name}

**Created:** {YYYY-MM-DD}
**Last updated:** {YYYY-MM-DD}
**Extracted from:** {description of the original work this came from}
**Slash command:** `/{process-name}`
**Times run:** 0

## When to Use

{1-2 sentences: what situation triggers this process. Be specific — when it applies AND when it does not.}

## Inputs Required

- {input 1}: {description}
- {input 2}: {description}

## Steps

### 1. {Step Name}

**What:** {action to take}
**Why:** {what this accomplishes}
**Inputs:** {what you need}
**Output:** {what this step produces}
**Judgment calls:** {where human decisions are needed, or "none"}

### 2. {Step Name}

...

## Expected Output

{Description of the final output: format, length, structure, tone}

## Common Pitfalls

{Things that go wrong, learned from experience}

- {pitfall 1}: {how to avoid}
- {pitfall 2}: {how to avoid}

## Related Processes

- {related process}: {relationship — run before/after/instead}
```

## File 7: `process-library/_gold-standard-template.md`

```
# Gold Standard: {Process Name}

**Source:** {what real work this was extracted from}
**Date:** {when the original work was done}

## Exemplar

The critical excerpt from the original output — the 20-30% that best demonstrates the pattern. This is a real artifact, not a hypothetical.

{Include the curated excerpt here — the most representative portion of the actual output. Not the whole thing, just the part that shows the standard.}

## Decision Log

Key decisions and tradeoffs that shaped the output. These are what make the gold standard useful — they capture the judgment, not just the result.

### Decision 1: {What was chosen}

- **Alternatives considered:** {what else could have been done}
- **Why this choice:** {the reasoning}
- **What would change it:** {conditions under which you'd choose differently}

### Decision 2: {What was chosen}

- **Alternatives considered:** {options}
- **Why this choice:** {reasoning}
- **What would change it:** {conditions}

{3-6 decisions is typical}

## Anti-Patterns

Concrete things that would make this output BAD. More useful than praise — they give specific things to avoid.

### 1. {Anti-pattern name}

- **What it looks like:** {concrete example of the bad output}
- **Why it's bad:** {the consequence}
- **What to do instead:** {the fix}

### 2. {Anti-pattern name}

- **What it looks like:** {example}
- **Why it's bad:** {consequence}
- **What to do instead:** {fix}

{3-5 anti-patterns is typical}
```

## File 8: `process-library/_checklist-template.md`

```
# Quality Checklist: {Process Name}

**Passing criteria:** All must-haves pass + at least 2/3 should-haves pass.

Every item is binary: PASS or FAIL. No partial credit. Each judgment requires a one-line evidence citation — quote the specific part of the output that proves it. No evidence = FAIL.

## Must-Have

These must ALL pass. If any fails, the output needs revision before use.

- [ ] {Concrete, verifiable yes/no question}
- [ ] {Concrete, verifiable yes/no question}
- [ ] {Concrete, verifiable yes/no question}
- [ ] {Concrete, verifiable yes/no question}

{4-5 items. Each should be unambiguous — a stranger could check it.}

## Should-Have

Most of these should pass. Failure means below-standard but still usable.

- [ ] {Concrete, verifiable yes/no question}
- [ ] {Concrete, verifiable yes/no question}
- [ ] {Concrete, verifiable yes/no question}

{3-4 items.}

## Nice-to-Have

Bonus quality. These distinguish good from great.

- [ ] {Concrete, verifiable yes/no question}
- [ ] {Concrete, verifiable yes/no question}

{2-3 items.}
```

## After creating all files

1. Confirm all files were created by listing them
2. Tell me what slash commands are now available
3. Explain that the process library is empty and I should run `/extract-process` after completing my next piece of work to start building it
