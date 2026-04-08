# Run Process

You are executing a named process from the process library and evaluating output against its gold standard.

## Arguments

`$ARGUMENTS` — first word is the process name, remaining words are context about what to apply it to.

Example: `/run-process api-endpoint-scaffold for the user notifications endpoint`

## Step 1: Load the Process

Read these files:
1. `process-library/{process-name}/process.md`
2. `process-library/{process-name}/gold-standard.md`
3. `process-library/{process-name}/evaluation-criteria.md`

If the process doesn't exist, tell the user and run `/list-processes`.

## Step 2: Internalize the Gold Standard

Before starting work, study the gold standard carefully. Note:
- The structure and format of excellent output
- The level of detail and specificity
- The tone and voice
- What the annotations say about WHY it's good

This is your target quality bar.

## Step 3: Execute the Process Steps

Follow `process.md` step by step:
1. Read each step's description
2. Gather the specified inputs
3. Do the work
4. Produce the specified output
5. **Quality check per step**: Does your output for this step match the quality shown in the gold standard for equivalent sections? If not, improve before moving on.

If a step requires human judgment, pause and ask.

## Step 4: Self-Evaluate Against the Gold Standard

After producing the complete output, score it honestly using the evaluation criteria.

Present the evaluation:

```
## Output Evaluation

| Dimension | Score (1-5) | Rationale |
|-----------|-------------|-----------|
| {dim 1}   | {score}     | {why — reference specific parts of your output} |
| {dim 2}   | {score}     | {why} |
| ...       | ...         | ... |
| **Total** | **{sum}** / {max} | |

Passing score: {from criteria} | Your score: {sum}
Target score: {from criteria} | Status: {PASS / BELOW TARGET / FAIL}
```

## Step 5: Structural Comparison

Compare your output against the gold standard:

1. **Structure match**: Same sections/components? Note missing or extra sections.
2. **Depth match**: Same level of detail? Note where you went deeper or shallower.
3. **Quality delta**: For the 1-2 weakest dimensions, quote the gold standard's equivalent section and yours side-by-side, explaining the gap.

## Step 6: Revise if Needed

**Below target score**: Identify the 1-2 weakest dimensions, revise those sections using the gold standard as reference, re-score, present both original and revised with scores.

**Below passing score**: Flag to the user: "Output is below minimum quality. Recommend revision before using." Offer to revise or let the user decide.

## Step 7: Save Run Record (Optional)

If tracking is desired, save to `process-library/{name}/runs/{YYYY-MM-DD}-{brief-description}.md`:

```
Date: {date}
Context: {what this was applied to}
Score: {X}/{Y} ({STATUS})
Strengths: {what scored well}
Gaps: {what scored low}
Revised: {yes/no}
```

## Output

Present to the user:
1. The complete process output
2. The evaluation table
3. The structural comparison with the gold standard
4. If revised: the revision and updated score

End with: "Process `{name}` complete. Score: {X}/{Y} ({STATUS})."
