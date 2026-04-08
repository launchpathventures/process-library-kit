# List Processes

Browse the process library.

## Process

1. Read `process-library/README.md` for the index
2. If README is stale, scan `process-library/*/process.md` to build the list dynamically
3. For each process, extract from `process.md`:
   - **Name**: Process name
   - **Description**: One-line summary (from "When to Use")
   - **Steps**: Number of steps
   - **Last used**: Most recent file in `runs/`, or "never"
   - **Gold standard**: What the exemplar was based on

Present as:

```
## Process Library

| Process | Description | Steps | Last Used | Gold Standard |
|---------|-------------|-------|-----------|---------------|
| {name}  | {desc}      | {n}   | {date}    | {source}      |
```

If the library is empty:

"No processes yet. After completing work, run `/extract-process` to capture it."

After listing, offer:
- `/run-process {name}` — execute a process
- `/extract-process` — capture a new one from recent work
