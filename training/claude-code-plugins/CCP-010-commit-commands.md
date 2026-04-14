---
type: procedure
created: 2026-03-20
status: active
slug: ccp-010-commit-commands
tags: [procedure, nexus]
---

# CCP-010 — Streamlined Git Commit/Push/PR Workflow

## Problema
Git workflows require multiple manual commands — status, diff, add, commit, push, PR creation — causing context switching that disrupts development flow. Commit messages are often generic or inconsistent with repository style, and branch cleanup after PR merges is frequently neglected.

## Procedura

### Prerequisites
- Git repository with changes staged or unstaged
- For `/commit-push-pr`: GitHub CLI (`gh`) installed (`brew install gh`) and authenticated (`gh auth login`)
- Install: `/plugin install commit-commands@claude-plugins-official`

### Steps

1. **Quick commit with auto-generated message**:
   ```
   /commit
   ```
   Claude runs `git status`, reviews both staged and unstaged changes, examines recent commit messages to match repository style, drafts an appropriate message, stages relevant files, and creates the commit. No manual `git add` needed.

2. **What `/commit` does NOT do**: Does not push or create a PR. Use for committing during active development when you want to checkpoint work.

3. **Commit safety features built in**:
   - Avoids staging files that likely contain secrets (`.env`, `credentials.json`)
   - Follows conventional commit practices
   - Matches existing commit message style from repo history
   - Includes Claude Code attribution in commit message

4. **Full workflow — commit + push + PR in one command**:
   ```
   /commit-push-pr
   ```
   Claude: creates a new branch if currently on main → stages and commits → pushes to origin → creates PR with `gh pr create` → provides the PR URL.

5. **What `/commit-push-pr` generates for the PR**:
   - Title: concise summary of changes
   - Body with: Summary (1-3 bullet points), Test plan checklist, Claude Code attribution
   - Analysis covers the full branch history (not just the latest commit)

6. **PR requirements**: `gh` must be installed and authenticated. Repository must have a remote named `origin` with GitHub as the remote.

7. **Branch cleanup after PRs are merged**:
   ```
   /clean_gone
   ```
   Claude lists all local branches, identifies those marked as `[gone]` (remote deleted), removes associated worktrees, and deletes the stale local branches. Run after merging multiple PRs.

8. **`/clean_gone` prerequisites**: Run `git fetch --prune` first if branches don't show as `[gone]`. The command is safe — it only removes branches already deleted from the remote.

9. **Recommended workflow**:
   ```
   # During development (multiple commits)
   /commit         # Checkpoint 1
   /commit         # Checkpoint 2

   # Ready to ship
   /commit-push-pr # Creates branch + commit + push + PR in one step

   # After several PRs merge
   /clean_gone     # Clean local branch list
   ```

10. **Troubleshooting**:
    - Empty commit: ensure changes exist (`git status`)
    - PR creation fails: install `gh` and run `gh auth login`
    - No `[gone]` branches: run `git fetch --prune` first

### Verification
- [ ] `/commit` creates a commit without requiring manual `git add`
- [ ] Commit message matches the style of recent repository commits
- [ ] `/commit-push-pr` creates a branch if currently on main
- [ ] PR body includes summary bullets and a test plan checklist
- [ ] `/clean_gone` only deletes branches already removed from remote

## Cortex Logging
- collection: procedures
- tags: claude-code-plugins, training, git, commit, pull-request, workflow, v2

## Enforcement Loop
- WHERE: Any git repository with Claude Code active
- WHEN: Ready to checkpoint work, create a PR, or clean up merged branches
- HOW: Use `/commit` during work, `/commit-push-pr` when ready for review, `/clean_gone` for maintenance
- CONNECT: CCP-002 (code-review runs on the PR created by this workflow)
