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

Example: "Anti-pattern: Adding validation after business logic. Why: Input could corrupt state before validation catches it. Instead: Validate at the top of the handler before any side effects."

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

```markdown
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
```

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

```
Process extracted: `{name}`

Files created:
- `process-library/{name}/process.md` — {N} steps
- `process-library/{name}/gold-standard.md` — exemplar + {N} decisions + {N} anti-patterns
- `process-library/{name}/checklist.md` — {N} checks ({N} must-have, {N} should-have, {N} nice-to-have)
- `.claude/commands/{name}.md` — invoke directly with `/{name}`

To run this process: `/{name} [context about what to apply it to]`
To browse all processes: `/list-processes`
```
