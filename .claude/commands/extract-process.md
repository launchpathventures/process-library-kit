# Extract Process

You are analyzing recently completed work to extract a repeatable process for the process library.

## Gather Context

Before anything, collect evidence of what just happened. Use whichever sources are available:

1. **Check if this is a git repo**: Run `git status`. If it works, also run `git diff HEAD~3` and `git log --oneline -10` to see what changed and in what order. If git is not available, skip this — the other sources are sufficient.
2. **Read key files**: Inspect the most important files that were created or modified during this session.
3. **Conversation context**: Review what the user asked for and the steps you took together. This is often the richest source of information about the process.

## Step 1: Identify the Process

Ask the user:
- What type of work was this? (e.g., "writing a blog post", "competitive research", "building an API endpoint", "creating a proposal")
- Would you do this type of work again?
- What would you name it?

Propose a kebab-case name: `blog-post-draft`, `competitive-research`, `api-endpoint-scaffold`, `client-proposal`.

## Step 2: Extract the Steps

From the evidence gathered, reconstruct what happened:

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

**Include only genuine decisions where alternatives were considered.** Simple work might have 1-2 decisions. Complex work might have 5-6. Do NOT pad with obvious choices just to hit a number — filler degrades the gold standard.

### Section C: Anti-Patterns

List concrete things that would make this output BAD. These are more useful than vague praise because they give Claude specific things to avoid.

Format:
- **Anti-pattern**: {what bad looks like}
- **Why it's bad**: {the consequence}
- **What to do instead**: {the fix}

**Include only anti-patterns you've actually seen or that are realistic risks for this type of work.** If there's only 1-2 genuine anti-patterns, that's fine. Do NOT pad with generic warnings like "don't be vague" — they add noise.

## Step 4: Build the Quality Checklist

Create a **binary checklist** — each item is PASS or FAIL with no middle ground.

Organize into three tiers:

### Must-Have (all must pass, or output needs revision)
Items where failure means the output is not usable. 3-5 items.
- Each item must be a concrete, verifiable yes/no question
- A stranger should be able to check it without judgment calls
- BAD: "Is the writing clear?" (subjective)
- GOOD: "Does every claim have a source cited?" (verifiable)

### Should-Have (most should pass)
Items where failure means below standard but still usable. 2-4 items.

### Nice-to-Have (bonus quality)
Items that distinguish good from great. 1-3 items.

**Passing criteria:**
- All must-haves pass
- At least 2/3 of should-haves pass

**Why a checklist, not a 1-5 rubric:** Numeric self-scoring is unreliable — Claude will rationalize a 4 on everything. Binary pass/fail forces honest assessment: either the claim HAS a source cited, or it doesn't. No wiggle room.

## Step 5: Write the Files

Create the process directory and files:

1. `process-library/{name}/process.md` — use `process-library/_template.md`
2. `process-library/{name}/gold-standard.md` — three sections from Step 3
3. `process-library/{name}/checklist.md` — the quality checklist from Step 4

Update `process-library/README.md` to include the new process in the index table.

## Step 6: Generate the Slash Command

Create a dedicated slash command so the user can invoke this process directly.

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

## Feedback (do not skip)

After presenting output and evaluation, ask the user:

"**How well did this process fit what you needed?**
1. **Good fit** — steps matched, minor tweaks at most
2. **Partial fit** — some steps relevant but work diverged significantly
3. **Wrong process** — this was actually a different type of work"

**Good fit**: Ask about specific improvements — missing/unnecessary steps, better example, new tradeoffs, new anti-patterns, ambiguous checklist items. Update process files with anything identified.

**Partial fit**: Ask whether to update this process or extract a new one for the divergent work.

**Wrong process**: Offer to extract what was actually done as a new process via `/extract-process`.

Save a run record to `process-library/{process-name}/runs/` and update the run count.

## Output

Present:
1. Your complete output
2. The checklist evaluation (with evidence per item)
3. A one-line summary: "{N}/{total} checks passed. {Must-have status}. {Any notable decisions made.}"

Then run the feedback conversation. After feedback: "Process updated with your feedback. {Summary of changes, or 'No changes needed.'}."
```

## Step 7: Validate the Process Itself

Before saving, present the extracted process to the user for review. Show them:

1. **The steps** — "Here are the {N} steps I extracted. Do these match what we actually did? Anything missing or unnecessary?"
2. **The gold standard exemplar** — "I picked this section as the representative example. Is this the right part to highlight?"
3. **The checklist** — "Here are the quality checks. Can each of these be answered with a clear yes or no?"

**Do NOT skip this step.** A bad process that gets saved and reused is worse than no process. The user must confirm before files are written.

## Step 8: Save and Confirm

Only after the user approves (or you've incorporated their feedback), write all the files.

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
