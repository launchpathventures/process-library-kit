# Process Library Kit

**Teach Claude to remember how you work — and get better at it every time.**

When you finish a piece of work with Claude in Co-Work, this kit lets you save the approach so Claude can follow the same steps next time. Each saved process includes an example of what good output looks like and a checklist to make sure the quality stays high.

Over time, you build a personal library of "how I do things" that Claude knows and improves on.

> Also works with Claude Code (CLI). Same setup, same files.

## Get Started

You need a **Claude Pro, Team, or Enterprise account** and a **project folder** you share with Co-Work. That's it.

Open Co-Work with your project folder and paste this:

```
I'd like you to set up a Process Library in this project. This will let us save and reuse the approaches we develop together.

Please download the setup files by running this command:
git clone https://github.com/launchpathventures/process-library-kit.git /tmp/process-library-kit

If that command fails, try this alternative:
curl -L https://github.com/launchpathventures/process-library-kit/archive/refs/heads/main.zip -o /tmp/plk.zip && unzip -o /tmp/plk.zip -d /tmp && mv /tmp/process-library-kit-main /tmp/process-library-kit && rm /tmp/plk.zip

If BOTH commands fail, stop and tell me — do NOT try to create the files from memory.

If it worked, set up the project:
1. Create a .claude/commands/ directory in this project if it doesn't already exist
2. Copy these four files from /tmp/process-library-kit/.claude/commands/ into this project's .claude/commands/ directory: extract-process.md, run-process.md, list-processes.md, update-process-library.md
3. Copy the entire /tmp/process-library-kit/process-library/ directory into this project
4. If this project already has a CLAUDE.md file, add the contents of /tmp/process-library-kit/CLAUDE.md to the end. If not, copy it as a new file.
5. Delete /tmp/process-library-kit

When you're done, show me what files you created and what new slash commands are available.
```

Hit the copy button, paste into Co-Work, and Claude sets everything up.

---

## How It Works

### 1. You work with Claude as normal
Write a proposal, do research, build a feature, create a presentation — whatever you do.

### 2. Claude asks if you want to save the approach
After finishing significant work, Claude asks: *"Want me to save how we did that so we can use the same approach next time?"*

### 3. You save it with `/extract-process`
Claude reviews what you did together and captures:
- **The steps** — what was done, in what order
- **An example of good output** — the best part of what you just produced, with notes on the key decisions you made and common mistakes to avoid
- **A quality checklist** — specific yes/no checks to make sure the output is good
- **A shortcut command** — type `/{name}` to run this process anytime

### 4. Next time, just type `/{name}`
Claude follows the saved steps, uses the example as a quality reference, and runs the checklist. If anything falls short, it fixes the issue before showing you the result.

### 5. Claude asks for feedback after every run
This is the most important part. After delivering the output, Claude asks:

**"How did that work?"**
- **Worked well** — Claude asks if anything could be better for next time (a missing step, a new mistake to watch for, a better example). Quick — usually one or two small tweaks.
- **Mostly worked, but the work went in a different direction** — Claude helps you decide: update this process to cover the new direction, or save the new approach as its own process.
- **Wrong approach** — Claude offers to save what you actually did as a new process instead.

This keeps your library healthy. Good processes get refined. Different work becomes its own process instead of being forced into one that doesn't fit.

### 6. Your library grows and gets sharper
- Every run either improves an existing process or creates a new one
- Mistakes get added to the "watch out for" list
- Examples get replaced when better ones come along
- Checklists get clearer over time

### 7. The framework updates itself
Claude checks for updates whenever you run a process command. If a new version is available, you'll see a note at the end: *"A Process Library update is available. Run `/update-process-library` to update."*

Updates only touch the framework — your saved processes are never changed.

---

## Setup

### Option A: Let Claude set it up (recommended)

Open Co-Work with your project folder and paste the prompt from [Get Started](#get-started) above.

### Option B: Set it up yourself

1. **Download** this repo (green "Code" button → Download ZIP, or clone it)
2. **Copy** the `.claude/commands/` folder (4 files) and `process-library/` folder into your project
3. **Copy** `CLAUDE.md` into your project (or add its contents to your existing one)

---

## What Gets Saved

When you extract a process, Claude creates a folder with three files plus a shortcut command. Here's what they look like for two different types of work:

<details>
<summary><b>Example: Competitive Research</b></summary>

**The Steps** (`process.md`)
```markdown
# Process: Competitive Research

Slash command: /competitive-research

## When to Use
When evaluating a market or product category before making a build/buy/partner decision.
Not for quick feature comparisons — this is a structured deep dive.

## Steps
1. Define the research question and scope
2. Identify the top 5-8 players
3. Analyze each on the same dimensions
4. Build the comparison matrix
5. Write the recommendation with tradeoffs
```

**The Example & Lessons** (`gold-standard.md`)
```markdown
## Best Output Example
{The comparison matrix and recommendation section — not the full report}

## Key Decisions We Made
### Structured matrix over narrative comparison
- Could have done: Written a pros/cons narrative for each competitor
- Why we chose this: Forces apples-to-apples comparison, easier to spot gaps
- Would reconsider if: Fewer than 3 competitors — narrative is fine for 2

### Included a "wild card" outside the obvious category
- Could have done: Only analyzed direct competitors
- Why we chose this: The CRM analysis missed Notion until we added it
- Would reconsider if: Time-constrained to under 2 hours

## Mistakes to Avoid
### Listing features without scoring them
- Looks like: "Tool A has reporting. Tool B has reporting."
- Problem: No basis for comparison — every tool "has" most features
- Instead: Rate each as weak / adequate / strong

### Researching only what the vendor says
- Looks like: Feature lists copied from marketing pages
- Problem: Marketing overstates. Real capabilities differ.
- Instead: Check reviews, community forums, trial the product
```

**The Quality Checklist** (`checklist.md`)
```markdown
## Must Pass
- [ ] Research question is stated at the top
- [ ] At least 5 competitors are analyzed
- [ ] All competitors are evaluated on the same dimensions
- [ ] Recommendation includes at least one tradeoff or risk

## Should Pass
- [ ] At least one "wild card" from outside the obvious category
- [ ] Sources beyond vendor marketing are cited

## Bonus
- [ ] Comparison matrix included as a table
- [ ] Each competitor has a one-line "best for" summary
```

</details>

<details>
<summary><b>Example: API Endpoint (for developers)</b></summary>

**The Steps** (`process.md`)
```markdown
# Process: API Endpoint Scaffold

Slash command: /api-endpoint-scaffold

## When to Use
When creating a new REST API endpoint. Not for modifying existing endpoints.

## Steps
1. Define the route schema
2. Write the handler with validation-first pattern
3. Add error response formatting
4. Write tests (happy path + edge cases)
5. Update API docs
```

**The Example & Lessons** (`gold-standard.md`)
```markdown
## Best Output Example
{The handler function — not every file touched}

## Key Decisions We Made
### Zod over manual validation
- Could have done: Manual if/else checks, joi, yup
- Why we chose this: Co-locates schema with TypeScript types, better errors
- Would reconsider if: Project already uses joi everywhere

## Mistakes to Avoid
### Business logic before validation
- Looks like: Database queries before input is validated
- Problem: Invalid input can corrupt state
- Instead: Always validate at the top of the handler
```

**The Quality Checklist** (`checklist.md`)
```markdown
## Must Pass
- [ ] Input validation happens before any business logic
- [ ] Every error response includes a machine-readable error code
- [ ] At least one test covers the happy path end-to-end

## Should Pass
- [ ] Error messages are specific enough to diagnose without logs
- [ ] Response types are explicitly typed (no `any`)
```

</details>

---

## Tips

- **Start with your next piece of work.** Don't try to pre-build a library — extract from real work you just did.
- **Not everything needs saving.** Quick fixes, one-off tasks, and exploratory work don't need a process. Only save approaches you'll use again.
- **The feedback after each run is the most valuable part.** That's where processes actually improve. Don't skip it.
- **It's OK to start rough.** Your first extracted process won't be perfect. It gets better every time you use it and give feedback.

## License

MIT
