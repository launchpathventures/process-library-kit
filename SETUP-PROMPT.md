# Setup Prompt

Copy the prompt below and paste it into your Co-Work (or Claude Code) session.

---

```
Please set up a Process Library in this project using https://github.com/launchpathventures/process-library-kit — download the files from that repo and install them into this project. If you can't access the link, let me know. When you're done, show me what's available.
```

---

If Claude has trouble accessing the link, use this longer version instead:

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
