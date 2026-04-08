# Setup Prompt

Copy the prompt below and paste it into your Co-Work (or Claude Code) session.

---

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
