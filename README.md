# Process Library Kit

A drop-in toolkit for **Claude Co-Work** (and Claude Code CLI) that gives Claude the ability to **extract repeatable processes from your work**, **replay them with quality evaluation**, and **build a cumulative library** of how you get things done.

Every process includes a **gold standard** (curated exemplar + decision log + anti-patterns) and a **binary quality checklist** so Claude evaluates honestly instead of giving itself 4/5 on everything.

> Works with both **Co-Work** (claude.ai) and **Claude Code** (CLI). The setup is identical — Co-Work reads `.claude/commands/` and `CLAUDE.md` the same way Claude Code does.

## Get Started

Open a Co-Work session with your project folder and paste this:

```
Set up a Process Library in this project so we can extract and reuse repeatable processes from our work.

Run this command:
git clone https://github.com/launchpathventures/process-library-kit.git /tmp/process-library-kit

IMPORTANT: If the clone fails, stop and tell me. Do NOT make up the file contents.

If the clone succeeds, do the following:
1. Create the .claude/commands/ directory in this project if it doesn't exist
2. Copy these three files from /tmp/process-library-kit/.claude/commands/ into this project's .claude/commands/:
   - extract-process.md
   - run-process.md
   - list-processes.md
3. Copy the entire /tmp/process-library-kit/process-library/ directory into this project
4. If this project has a CLAUDE.md, append the contents of /tmp/process-library-kit/CLAUDE.md to the end. If not, copy it as a new file.
5. Remove /tmp/process-library-kit

After setup, list every file you created and show me the slash commands that are now available.
```

Hit the copy button in the top-right corner, paste into Co-Work, and Claude sets everything up.

---

## The Problem

You do great work with Claude, but:
- Next session starts from scratch — no memory of *how* you did it
- Similar tasks get approached differently each time
- Quality varies because there's no reference for "what good looks like"
- You can't build on past approaches

## The Solution

Extract processes from real work, then invoke them directly by name:

```
/extract-process                          # capture what you just did
/api-endpoint-scaffold for notifications  # run it again later (auto-generated command)
/list-processes                           # browse everything
```

Each extracted process automatically creates its own slash command — no need to remember names or use a generic runner.

## How It Works

### 1. You do work as normal
Build a feature, write docs, conduct research, fix a bug — whatever.

### 2. Claude suggests extraction
After completing significant work, Claude asks: *"This looks like a repeatable process. Want to capture it?"*

### 3. `/extract-process` captures the pattern
Claude analyzes the git diff and conversation to extract:

- **The steps** — what was done, in what order, with what inputs
- **The gold standard** — three parts:
  - **Exemplar**: A curated excerpt (the critical 20-30% of your output, not the whole thing)
  - **Decision log**: The 3-6 key tradeoffs that shaped the output ("we chose X over Y because Z")
  - **Anti-patterns**: 3-5 concrete things that would make the output BAD
- **Quality checklist** — binary pass/fail items (not a numeric rubric)
- **A slash command** — `/{process-name}` for direct invocation

### 4. Next time, `/{process-name}`
Claude loads the process, studies the gold standard, follows the steps, then:
- Runs every checklist item as PASS/FAIL with evidence citations
- Checks output against the anti-patterns list
- Auto-revises if any must-have check fails
- Saves a run record automatically
- **Asks you**: "Did this run reveal a missing step, a new anti-pattern, or a better example?"

### 5. Processes improve through use
After every run, Claude prompts you to capture what you learned. Over time:
- Anti-patterns accumulate from real failures
- Steps get added or removed based on what actually happens
- Gold standards get replaced when better examples emerge
- Checklist items sharpen when ambiguous ones get rewritten

---

## Prerequisites

Before setup, make sure you have:

### For Co-Work (claude.ai)

- **A Claude Pro, Team, or Enterprise account** — Co-Work is available on paid plans
- **A project folder** — a folder on your computer that you'll share with Co-Work. This is where your code, docs, or other work lives. It can be an existing project or a new empty folder.

That's it. No packages to install, no dependencies, no build steps.

### For Claude Code (CLI)

- **Claude Code installed** — [installation guide](https://docs.anthropic.com/en/docs/claude-code)
- **A project directory**
- Same requirements as above, but you're working in the terminal

---

## Setup

### Option A: Let Claude set it up (recommended)

Open a Co-Work session with your project folder and paste this:

```
Set up a Process Library in this project so we can extract and reuse repeatable processes from our work.

Run this command:
git clone https://github.com/launchpathventures/process-library-kit.git /tmp/process-library-kit

IMPORTANT: If the clone fails, stop and tell me. Do NOT make up the file contents.

If the clone succeeds, do the following:
1. Create the .claude/commands/ directory in this project if it doesn't exist
2. Copy these three files from /tmp/process-library-kit/.claude/commands/ into this project's .claude/commands/:
   - extract-process.md
   - run-process.md
   - list-processes.md
3. Copy the entire /tmp/process-library-kit/process-library/ directory into this project
4. If this project has a CLAUDE.md, append the contents of /tmp/process-library-kit/CLAUDE.md to the end. If not, copy it as a new file.
5. Remove /tmp/process-library-kit

After setup, list every file you created and show me the slash commands that are now available.
```

### Option B: Copy files manually

If you prefer to set things up yourself:

1. **Download or clone** this repo
2. **Copy** these into your project folder:
   - `.claude/commands/extract-process.md`
   - `.claude/commands/run-process.md`
   - `.claude/commands/list-processes.md`
   - `process-library/` (the whole directory)
3. **Add instructions** — append the contents of [`CLAUDE.md`](./CLAUDE.md) to your project's `CLAUDE.md` (or copy it if you don't have one yet)

### After setup

Your process library starts empty. The first time you finish a piece of work in Co-Work, Claude will suggest: *"This looks like a repeatable process. Want to capture it?"*

Or run `/extract-process` yourself anytime.

---

## What a Process Looks Like

Each extracted process creates four artifacts. Here are two examples — one for code, one for non-code work:

<details>
<summary><b>Example: Code — API Endpoint Scaffold</b></summary>

**`process.md`** — The Steps
```markdown
# Process: API Endpoint Scaffold

**Created:** 2026-04-08
**Extracted from:** Building the /api/v1/users endpoint
**Slash command:** `/api-endpoint-scaffold`

## When to Use
When creating a new REST API endpoint. NOT for modifying existing endpoints.

## Steps
### 1. Define the Route Schema
### 2. Write the Handler with Validation-First Pattern
### 3. Add Error Response Formatting
### 4. Write Tests (Happy Path + Edge Cases)
### 5. Update API Docs
```

**`gold-standard.md`** — The Reference
```markdown
## Exemplar
{The critical 20-30% of the actual output — just the handler function, not every file}

## Decision Log
### Decision 1: Zod over manual validation
- **Alternatives:** Manual if/else checks, joi, yup
- **Why Zod:** Co-locates schema with TypeScript types, better error formatting
- **Would change if:** Project already uses joi everywhere

## Anti-Patterns
### 1. Business logic before validation
- **What it looks like:** Database queries before input is validated
- **Why it's bad:** Invalid input can corrupt state
- **Instead:** Always validate at the top of the handler
```

**`checklist.md`** — The Quality Gate
```markdown
## Must-Have
- [ ] Input validation happens before any business logic or side effects
- [ ] Every error response includes a machine-readable error code
- [ ] At least one test covers the happy path end-to-end

## Should-Have
- [ ] Error messages are specific enough to diagnose without checking logs
- [ ] Response types are explicitly typed (no `any`)
```

</details>

<details>
<summary><b>Example: Non-code — Competitive Research</b></summary>

**`process.md`** — The Steps
```markdown
# Process: Competitive Research

**Created:** 2026-04-08
**Extracted from:** CRM landscape analysis for SMB market entry
**Slash command:** `/competitive-research`

## When to Use
When evaluating a market or product category before making a build/buy/partner decision.
NOT for quick feature comparisons — this is a structured deep dive.

## Steps
### 1. Define the Research Question and Scope
### 2. Identify the Top 5-8 Players
### 3. Analyze Each on the Same Dimensions
### 4. Build the Comparison Matrix
### 5. Write the Recommendation with Tradeoffs
```

**`gold-standard.md`** — The Reference
```markdown
## Exemplar
{The comparison matrix and recommendation section from the CRM analysis — not the full 20-page report}

## Decision Log
### Decision 1: Structured matrix over narrative comparison
- **Alternatives:** Write a pros/cons narrative for each competitor
- **Why matrix:** Forces apples-to-apples comparison, easier to spot gaps
- **Would change if:** Fewer than 3 competitors — narrative is fine for 2

### Decision 2: Include a "wild card" outside the obvious category
- **Alternatives:** Only analyze direct competitors
- **Why:** The CRM analysis missed Notion until we added it — adjacent tools often reveal positioning gaps
- **Would change if:** Time-constrained to under 2 hours

## Anti-Patterns
### 1. Listing features without scoring them
- **What it looks like:** "Tool A has reporting. Tool B has reporting."
- **Why it's bad:** No basis for comparison — every tool "has" most features
- **Instead:** Score each feature on a scale or rate as weak/adequate/strong

### 2. Researching only what the vendor says
- **What it looks like:** Feature lists copied from marketing pages
- **Why it's bad:** Marketing overstates. Real capabilities differ.
- **Instead:** Check reviews, community forums, and trial the product if possible
```

**`checklist.md`** — The Quality Gate
```markdown
## Must-Have
- [ ] Research question is explicitly stated at the top
- [ ] At least 5 competitors are analyzed
- [ ] All competitors are evaluated on the same dimensions
- [ ] Recommendation includes at least one tradeoff or risk

## Should-Have
- [ ] At least one "wild card" from outside the obvious category is included
- [ ] Sources beyond vendor marketing are cited (reviews, forums, trials)

## Nice-to-Have
- [ ] Comparison matrix is included as a table
- [ ] Each competitor has a one-line "best for" summary
```

</details>

### `.claude/commands/{name}.md` — The Slash Command

Auto-generated so you type `/{name}` directly instead of `/run-process {name}`.

---

## Why This Design

### Gold standard: curated excerpt + decisions + anti-patterns (not annotated dump)

**Problem with v1:** "Dump the whole output and annotate why it's good" doesn't work. Real outputs are hundreds of lines — Claude skims them. Annotations like `<!-- GOOD: clean separation -->` are vague and self-congratulatory.

**What actually helps:** The *decisions* that shaped the output, not the output itself. "We chose Zod over manual validation because..." is actionable. "This is good code" is not. The anti-patterns give specific guardrails. The exemplar is small enough to actually study.

### Checklist: binary pass/fail (not 1-5 rubric)

**Problem with v1:** Numeric self-scoring is grading your own homework. Claude will rationalize a 4 on everything because 3 feels harsh and 5 feels overconfident. The scores become meaningless.

**What actually works:** Binary forces honesty. "Does every endpoint validate input before processing?" — either it does or it doesn't. The evidence citation requirement ("quote the specific line") makes it impossible to hand-wave.

### Slash commands: auto-generated per process (not `/run-process {name}`)

**Problem with v1:** Users have to remember process names and type `/run-process api-endpoint-scaffold`. Friction kills adoption.

**What actually works:** Each process generates its own `.claude/commands/{name}.md`. Type `/{name}` directly. The process library becomes a growing set of slash commands that show up in Co-Work's and Claude Code's command palette.

---

## Tips

- **Start small**: Extract your first process from work you just did. Don't pre-build a library.
- **Real exemplars only**: Use actual output, not hypotheticals. Curate it down to the critical 20-30%.
- **Decision log is king**: This is the most valuable part of the gold standard. 3 good decisions beat 30 lines of annotated code.
- **Update anti-patterns from failures**: When a process run produces bad output, add the failure mode to the anti-patterns list.
- **Checklist items must be verifiable by a stranger**: If a checklist item requires subjective judgment, rewrite it until it's binary.
- **Not everything is a process**: One-off fixes, trivial tasks, and exploratory work don't need capturing.

## File Structure

```
your-project/
├── CLAUDE.md                              <- add the process library section
├── .claude/commands/
│   ├── extract-process.md                 <- /extract-process (core kit)
│   ├── run-process.md                     <- /run-process (generic runner)
│   ├── list-processes.md                  <- /list-processes
│   ├── api-endpoint-scaffold.md           <- auto-generated per process
│   └── competitive-research.md            <- auto-generated per process
└── process-library/
    ├── README.md                          <- library index
    ├── _template.md                       <- process definition template
    ├── _gold-standard-template.md         <- gold standard template
    ├── _checklist-template.md             <- quality checklist template
    └── {process-name}/
        ├── process.md
        ├── gold-standard.md
        ├── checklist.md
        └── runs/                          <- optional score history
```

## License

MIT
