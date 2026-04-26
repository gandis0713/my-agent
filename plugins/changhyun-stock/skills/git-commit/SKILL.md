---
name: git-commit
description: Analyze staged git changes and generate a commit message using Claude Haiku, then commit.
model: haiku
---

# Git Commit

Analyze staged git changes and automatically generate a concise commit message using Claude Haiku, then create the commit.

## Steps

### 1. Check Staged Changes

Run the following commands to inspect what is staged:

```bash
git status
git diff --cached
```

If there are no staged changes, stop and inform the user that there is nothing to commit.

### 2. Generate Commit Message with Claude Haiku

Capture the staged diff and pass it to Claude Haiku to generate a commit message:

```bash
git diff --cached | claude -m claude-haiku-4-5-20251001 -p "Write a git commit message for the following diff. Rules: (1) Subject line must be 72 characters or less, written in imperative mood (e.g. 'Add feature', not 'Added feature'). (2) Optionally follow with a blank line and a short body if the change needs explanation. (3) Output only the commit message — no extra commentary."
```

Capture the output as the commit message.

### 3. Confirm with User

Display the generated commit message to the user and ask for confirmation before committing.

### 4. Create the Commit

Run git commit with the approved message:

```bash
git commit -m "<generated message>"
```

If the message has multiple lines (subject + body), use a heredoc:

```bash
git commit -m "$(cat <<'EOF'
<subject line>

<body>
EOF
)"
```

## Notes

- Only staged files (added via `git add`) are used as context for the message.
- Do not stage or unstage any files as part of this skill.
- If the diff is very large (over 500 lines), summarize the most significant changes rather than passing the entire diff.
- Do not skip pre-commit hooks (`--no-verify`).
