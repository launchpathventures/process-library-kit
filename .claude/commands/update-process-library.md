# Update Process Library

Check for and install updates to the Process Library Kit framework.

## Step 1: Check Current Version

Read `process-library/VERSION` to get the current local version number.

## Step 2: Check for Updates

Run this command to get the latest version:
```
curl -s https://raw.githubusercontent.com/launchpathventures/process-library-kit/main/process-library/VERSION
```

If the command fails (network issue), tell the user: "Couldn't check for updates — no internet access. You can check manually at https://github.com/launchpathventures/process-library-kit"

## Step 3: Compare

If the remote version is the same as the local version, tell the user: "You're on the latest version (v{version}). No updates available."

If the remote version is higher, tell the user: "An update is available (you have v{local}, latest is v{remote}). Want me to update?"

## Step 4: Download (if user agrees)

Run:
```
git clone https://github.com/launchpathventures/process-library-kit.git /tmp/process-library-kit-update
```

If that fails, try:
```
curl -L https://github.com/launchpathventures/process-library-kit/archive/refs/heads/main.zip -o /tmp/plk-update.zip && unzip -o /tmp/plk-update.zip -d /tmp && mv /tmp/process-library-kit-main /tmp/process-library-kit-update && rm /tmp/plk-update.zip
```

If BOTH fail, stop and tell the user. Do NOT make up file contents.

## Step 5: Check for CLAUDE.md Changes

BEFORE doing anything else, compare the new CLAUDE.md with the user's current one. If there are new sections or significant changes, note them — you'll tell the user about these after the update.

## Step 6: Install Update

Update ONLY the framework files (not the user's processes or CLAUDE.md):
1. Replace `.claude/commands/extract-process.md` with the new version
2. Replace `.claude/commands/run-process.md` with the new version
3. Replace `.claude/commands/list-processes.md` with the new version
4. Replace `.claude/commands/update-process-library.md` with the new version
5. Replace `process-library/VERSION` with the new version
6. Replace `process-library/_template.md` with the new version
7. Replace `process-library/_gold-standard-template.md` with the new version
8. Replace `process-library/_checklist-template.md` with the new version

Do NOT touch:
- The user's CLAUDE.md (they may have customized it)
- Any process directories (`process-library/*/`) — these are the user's processes
- Any auto-generated slash commands (`.claude/commands/{process-name}.md`)
- `process-library/README.md` (the user's index)

## Step 7: Clean Up

```
rm -rf /tmp/process-library-kit-update
```

## Step 8: Report

Tell the user: "Updated to v{version}. Your processes and settings are untouched — only the framework commands and templates were updated."

If there were CLAUDE.md changes (from Step 5):
"The framework's instructions file has new content. Want me to show you what changed so you can decide whether to add it to yours?"

Let the user decide — never auto-modify their CLAUDE.md.
