# Extract Process

You are analyzing recently completed work to extract a repeatable process for the process library.

## Gather Context

Before anything, collect evidence of what just happened:

1. **Git diff**: Run `git diff HEAD~3` (adjust range if needed) to see what changed
2. **Git log**: Run `git log --oneline -10` for the sequence of commits
3. **Read key files**: Inspect the most important files that were created or modified
4. **Conversation context**: Consider what was asked for and the steps you took

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

## Step 3: Create the Gold Standard

The work just completed IS the gold standard (or its basis). Extract the most representative output — the file, document, code, or artifact that best shows "what good looks like."

Rules:
- Must be a real artifact from real work, not a hypothetical
- Should be the OUTPUT of the process, not the process itself
- Include enough context that someone unfamiliar could understand why it's good
- **Annotate it**: Add brief inline comments explaining what makes each section good (use `<!-- GOOD: reason -->` comments or a "Why this is good" section)

If the output had flaws, note them honestly. A gold standard with acknowledged caveats is better than a dishonest one.

## Step 4: Define Evaluation Criteria

Create a scoring rubric with 4-6 dimensions. Each dimension:
- Has a 1-5 scale with concrete descriptions for levels 1, 3, and 5
- Is specific to THIS type of work

Choose dimensions that fit (don't use all):
- **Completeness**: Are all required elements present?
- **Correctness**: Is content factually/technically accurate?
- **Clarity**: Is the output easy to understand?
- **Consistency**: Does it follow conventions?
- **Specificity**: Are claims concrete and evidenced, or vague?
- **Actionability**: Can someone act on this without further clarification?
- **Scope discipline**: Does it stay within bounds?

Define:
- **Minimum passing score**: Below this, output must be revised
- **Target score**: What good process execution achieves

## Step 5: Write the Files

Create three files using the templates in `process-library/`:

1. `process-library/{name}/process.md` — use `process-library/_template.md`
2. `process-library/{name}/gold-standard.md` — the annotated exemplar
3. `process-library/{name}/evaluation-criteria.md` — the rubric from `process-library/_evaluation-rubric.md`

Update `process-library/README.md` to include the new process in the index table.

## Step 6: Verify

Read all three files back and check:
- Could someone unfamiliar follow this process?
- Is the gold standard genuinely good?
- Are evaluation criteria concrete enough to score without debate?
- Does the process match what was ACTUALLY done, not what you wish was done?

## Output

Tell the user:

```
Process extracted: `{name}`

- `process-library/{name}/process.md` — {N} steps
- `process-library/{name}/gold-standard.md` — based on {source artifact}
- `process-library/{name}/evaluation-criteria.md` — {N} dimensions, passing score {X}/{Y}

To run: `/run-process {name}`
To browse: `/list-processes`
```
