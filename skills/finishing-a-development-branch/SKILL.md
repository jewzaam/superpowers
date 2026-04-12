---
name: finishing-a-development-branch
description: Use when implementation is complete, all tests pass, and you need to wrap up a development branch - verifies tests pass and reports completion status
allowed-tools: Read Bash(git status *) Bash(git log *) Bash(git branch --show-current) Bash(git merge-base *)
---

# Finishing a Development Branch

## Overview

Guide completion of development work by verifying tests pass and reporting status.

**Core principle:** Verify tests → Report completion. The user manages merges, PRs, and branch cleanup themselves.

**Announce at start:** "I'm using the finishing-a-development-branch skill to complete this work."

## The Process

### Step 1: Verify Tests

**Before presenting options, verify tests pass:**

```bash
# Run project's test suite
npm test / cargo test / pytest / go test ./...
```

**If tests fail:**
```
Tests failing (<N> failures). Must fix before completing:

[Show failures]

Cannot proceed with merge/PR until tests pass.
```

Stop. Don't proceed to Step 2.

**If tests pass:** Continue to Step 2.

### Step 2: Determine Base Branch

```bash
# Try common base branches
git merge-base HEAD main 2>/dev/null || git merge-base HEAD master 2>/dev/null
```

If the merge-base check fails, use AskUserQuestion to confirm the base branch.

### Step 3: Report Completion

Report that implementation is complete with a summary of what was done:

```
Implementation complete on branch <name>.

Summary:
- <what was built/changed>
- <test results>
- Branch: <name>
- Worktree: <path> (if applicable)
```

Do NOT present options for merging, creating PRs, or discarding work. The user manages branch integration and cleanup themselves. Simply report completion status and stop.

## Preferred Tools

Use these tools to complete this skill's work. Deviating from this set may trigger permission prompts, which slow execution and degrade the experience. Stick to these unless you have no alternative:

- **Read** — check project files
- **Bash**: `git status`, `git log`, `git branch --show-current`, `git merge-base` — assess branch state

If you need a tool not on this list, proceed — but understand it may require user approval.

## Common Mistakes

**Skipping test verification**
- **Problem:** Report completion with broken code
- **Fix:** Always verify tests before reporting

**Presenting integration options**
- **Problem:** Asking user to choose merge/PR/keep/discard wastes time
- **Fix:** Report completion and stop. The user handles integration.

## Red Flags

**Never:**
- Proceed with failing tests
- Merge, push, or create PRs without explicit user request
- Delete branches or worktrees without explicit user request

**Always:**
- Verify tests before reporting completion
- Report branch name and worktree path
- Stop after reporting — let the user decide next steps

## Integration

**Called by:**
- **subagent-driven-development** (Step 7) - After all tasks complete
- **executing-plans** (Step 5) - After all batches complete

**Pairs with:**
- **using-git-worktrees** - Reports worktree path so the user can manage cleanup
