---
name: end-session-checkpoint
description: "Create strong end-of-session checkpoints and handoff notes for coding work. Use when the user wants to end a session, wrap up work, create a restart note, hand off to the next agent, summarize repo state, or save current status."
license: MIT
---

# End Session Checkpoint

## Workflow

Copy this checklist and check off items as you complete them:

```text
End Session Checkpoint Progress:

- [ ] Step 1: Inspect repo state
  - [ ] 1.1 Run git status and capture clean/dirty state
  - [ ] 1.2 Capture diff shape and complexity signals
  - [ ] 1.3 Check whether prior context file exists
  - [ ] 1.4 Pull changes from session history or your memory

- [ ] Step 2: Run exit interview and collect information
  - [ ] 2.1 Collect goal, decisions, gotchas, verification, next steps
  - [ ] 2.2 Ask commit question only if repo is dirty
- [ ] Step 3: Execute actions
  - [ ] 3.1 Commit or push only if user said yes
  - [ ] 3.2 Archive old context file locally
- [ ] Step 4: Write checkpoint
  - [ ] 4.1 Synthesize a strong restart note
  - [ ] 4.2 Scale depth to repo complexity
  - [ ] 4.3 Save to .agents/context/CURRENT_STATUS.md
- [ ] Step 5: Review output before delivery
```

## Step 1: Inspect Repo State

Run these commands silently:

- `git status`
- `git diff --stat`
- `git status --porcelain`
- `git log -5 --oneline --decorate` (optional)

## Step 2: Run Exit Interview

Always run the interview, even if the repo is clean.

Collect the minimum information needed for a real handoff:
- Next steps in priority order
- Any other questions you have about vague decisions made during the session

If the repo is dirty, also ask:

- Commit recent changes to git? Yes or no.
- Push to remote? Yes or no.

## Step 3: Execute Actions

### 3.1 Commit and Push (conditional)

Only commit or push if the user explicitly said yes.

- Generate a concise conventional commit message from the diff.
- Run normal git hooks.

Prefer reviewed staging over blind staging when the worktree contains suspicious files.

### 3.2 Archive Prior Context

If `.agents/context/CURRENT_STATUS.md` exists, move it to:

- `.agents/context/archive/[TIMESTAMP]-session-summary.md`

Do this after any commit so the archival step stays local.

## Step 4: Write Checkpoint

Write a new `.agents/context/CURRENT_STATUS.md`.

Do not fill a rigid template.

Write one sharp checkpoint for the next agent. Use headings only when they help clarity. The exact structure may change with the session, but the checkpoint must cover these questions:

- What was the session trying to accomplish?
- What now works?
- What is still incomplete, broken, risky, or unverified?
- Where should the next agent start first?
- Which files, commands, or entrypoints matter most?
- Which decisions matter later, and why?
- What should not be repeated or accidentally undone?

## Step 5: Review Output Before Delivery

Ask:

- If I were dropped into this repo cold, could I restart from this note without reading the full transcript?
- Does the note tell me what to do first, not just what happened?
- Are all important claims tied to user input, repo state, or actual verification?
- Did I mark unknowns honestly instead of bluffing?

If the answer to any of those is no, rewrite the checkpoint before delivering it.
