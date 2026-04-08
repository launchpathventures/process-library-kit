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

```
## Process Library

| Command | Description | Steps | Checks | Last Used |
|---------|-------------|-------|--------|-----------|
| `/{name}` | {desc} | {n} | {n} | {date} |
```

If the library is empty:

"No processes yet. After completing work, run `/extract-process` to capture it."

After listing:
- "Run any process directly: `/{name} [context]`"
- "Or use the generic runner: `/run-process {name} [context]`"
- "Extract a new process: `/extract-process`"
