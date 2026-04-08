# Process Library Kit for Claude Code

A drop-in toolkit that gives Claude Code the ability to **extract repeatable processes from your work**, **replay them with quality evaluation**, and **build a cumulative library** of how you get things done.

Every process includes a **gold standard** — a real example of excellent output — and a **scoring rubric** so Claude can evaluate its work against what good actually looks like.

## The Problem

You do great work with Claude Code, but:
- Next session starts from scratch — no memory of *how* you did it
- Similar tasks get approached differently each time
- Quality varies because there's no reference for "what good looks like"
- You can't build on past approaches

## The Solution

Three slash commands that turn Claude Code into a process-aware collaborator:

| Command | What it does |
|---------|-------------|
| `/extract-process` | After finishing work, analyze what was done and capture it as a reusable process |
| `/run-process {name}` | Execute a saved process with gold-standard quality evaluation |
| `/list-processes` | Browse your process library |

## How It Works

### 1. You do work as normal
Build a feature, write docs, conduct research, fix a bug — whatever.

### 2. Claude suggests extraction
After completing significant work, Claude asks: *"This looks like a repeatable process. Want to capture it?"*

### 3. `/extract-process` captures the pattern
Claude analyzes the git diff and conversation to extract:
- **The steps** — what was done, in what order, with what inputs
- **The gold standard** — the actual output you just produced, annotated with why it's good
- **Evaluation criteria** — a 4-6 dimension scoring rubric with concrete 1-5 descriptions

### 4. Next time, `/run-process {name}`
Claude loads the process, studies the gold standard, follows the steps, then:
- **Scores its own output** against the rubric (1-5 per dimension)
- **Compares structurally** against the gold standard (same sections? same depth?)
- **Auto-revises** if below target score
- **Saves a run record** for quality tracking over time

### 5. The library grows and improves
- Gold standards get replaced when better examples emerge
- Steps refine as runs reveal gaps
- Run records show quality trends
- Only processes worth repeating get captured

---

## Setup (5 minutes)

### Step 1: Copy the files into your project

```bash
# Clone this repo
git clone https://github.com/launchpathventures/process-library-kit.git

# Copy into your project
cp -r process-library-kit/.claude/commands/extract-process.md YOUR_PROJECT/.claude/commands/
cp -r process-library-kit/.claude/commands/run-process.md YOUR_PROJECT/.claude/commands/
cp -r process-library-kit/.claude/commands/list-processes.md YOUR_PROJECT/.claude/commands/
cp -r process-library-kit/process-library YOUR_PROJECT/
```

Or just copy the directories manually:
- `.claude/commands/` — the three slash command files
- `process-library/` — the library home with templates

### Step 2: Add the CLAUDE.md section

Append the contents of [`CLAUDE.md`](./CLAUDE.md) from this repo to your project's `CLAUDE.md`.

If your project doesn't have a `CLAUDE.md` yet, copy the whole file:

```bash
cp process-library-kit/CLAUDE.md YOUR_PROJECT/CLAUDE.md
```

### Step 3: Start working

That's it. Next time you finish a piece of work, Claude will suggest running `/extract-process`. Or run it yourself anytime:

```
/extract-process
```

---

## What a Process Looks Like

Each extracted process lives in `process-library/{name}/` with three files:

### `process.md` — The Definition

```markdown
# Process: API Endpoint Scaffold

**Created:** 2026-04-08
**Extracted from:** Building the /api/v1/users endpoint

## When to Use
When creating a new REST API endpoint that follows our standard patterns.

## Steps
### 1. Define the Route
### 2. Write the Handler
### 3. Add Validation
### 4. Write Tests
### 5. Update API Docs
```

### `gold-standard.md` — What Good Looks Like

A real, annotated example of excellent output from the original work. Not a hypothetical — the actual artifact, with comments explaining why each part is good.

```markdown
# Gold Standard: API Endpoint Scaffold

Source: /api/v1/users endpoint (built 2026-04-08)

## The Output

<!-- GOOD: Input validation happens before any business logic -->
const schema = z.object({
  email: z.string().email(),
  name: z.string().min(1).max(100),
});

<!-- GOOD: Error responses use consistent format with actionable messages -->
if (!result.success) {
  return Response.json({ error: result.error.format() }, { status: 400 });
}
```

### `evaluation-criteria.md` — The Scoring Rubric

```markdown
# Evaluation Criteria: API Endpoint Scaffold

**Passing score:** 15 / 25
**Target score:** 20 / 25

### 1. Input Validation
| Score | Description |
|-------|-------------|
| 1     | No validation, raw input passed to business logic |
| 3     | Basic type checking, but missing edge cases |
| 5     | Comprehensive schema validation with clear error messages |

### 2. Error Handling
...
```

---

## Evaluation in Action

When you run `/run-process api-endpoint-scaffold`, Claude produces the output then self-evaluates:

```
## Output Evaluation

| Dimension        | Score (1-5) | Rationale                                    |
|------------------|-------------|----------------------------------------------|
| Input Validation | 4           | Schema covers all fields, missing one edge case |
| Error Handling   | 5           | Matches gold standard format exactly          |
| Test Coverage    | 3           | Happy path covered, missing error scenarios   |
| Documentation    | 4           | OpenAPI spec complete, examples could be richer |
| **Total**        | **16** / 20 |                                               |

Target: 17/20 | Status: BELOW TARGET

Revising test coverage and documentation sections...
```

Claude then revises the weak areas using the gold standard as reference and re-scores.

---

## Tips

- **Start small**: Extract your first process from work you just did. Don't try to pre-build a library.
- **Real gold standards only**: Hypothetical examples don't anchor quality. Use actual output.
- **Update as you go**: After a few runs, you'll notice steps that are missing or unnecessary. Edit the process.
- **Not everything is a process**: One-off fixes, trivial tasks, and exploratory work don't need capturing. Only extract patterns you'll repeat.
- **Run records are optional**: Start without them. Add tracking later for processes you run frequently and want to monitor quality trends.

## File Structure

```
your-project/
├── CLAUDE.md                              <- add the process library section
├── .claude/commands/
│   ├── extract-process.md                 <- /extract-process
│   ├── run-process.md                     <- /run-process
│   └── list-processes.md                  <- /list-processes
└── process-library/
    ├── README.md                          <- library index (auto-maintained)
    ├── _template.md                       <- process definition template
    ├── _evaluation-rubric.md              <- scoring rubric template
    └── {process-name}/                    <- one directory per process
        ├── process.md
        ├── gold-standard.md
        ├── evaluation-criteria.md
        └── runs/                          <- optional score history
            └── 2026-04-08-first-run.md
```

## License

MIT
