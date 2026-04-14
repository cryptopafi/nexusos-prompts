---
type: procedure
created: 2026-03-20
status: active
slug: ccp-002-code-review
tags: [procedure, nexus]
---

# CCP-002 — Multi-Agent PR Review with Confidence Scoring

## Problema
Manual PR review is time-consuming, inconsistent, and prone to missing subtle bugs or guideline violations. A single reviewer can't simultaneously check for bugs, compliance with project conventions, and historical context, leading to issues slipping into production.

## Procedura

### Prerequisites
- Git repository with GitHub integration
- GitHub CLI (`gh`) installed and authenticated: `gh auth login`
- CLAUDE.md files in the repository (optional but strongly recommended)
- Install: `/plugin install code-review@claude-plugins-official`

### Steps

1. **Run the command on any PR branch**:
   ```
   /code-review
   ```
   Claude automatically detects the current PR context.

2. **Automatic skip conditions**: Claude checks before running and skips if the PR is closed, marked as draft, trivial/automated, or already has a review from Claude. No manual filtering required.

3. **CLAUDE.md gathering**: Claude scans the repository for all CLAUDE.md guideline files at all directory levels. These form the baseline for compliance checking. Better CLAUDE.md files = better reviews.

4. **PR summarization**: Claude reads the full diff and summarizes the changes to establish context for all review agents.

5. **4-agent parallel launch**: Claude spawns four independent review agents simultaneously:
   - **Agent #1**: CLAUDE.md compliance audit — checks each change against project guidelines
   - **Agent #2**: Second CLAUDE.md compliance audit — redundancy reduces false negatives
   - **Agent #3**: Bug scanner — focuses only on bugs introduced in this PR (not pre-existing)
   - **Agent #4**: Historical context analyzer — uses `git blame` to understand prior intent and catch context-dependent issues

6. **Confidence scoring per issue**: Each identified issue is independently scored 0-100:
   - 0 = not confident, likely false positive
   - 25 = somewhat confident
   - 50 = moderately confident
   - 75 = highly confident, real and important
   - 100 = absolutely certain

7. **Filtering**: Issues below the 80-confidence threshold are discarded. False positive categories that are always filtered: pre-existing issues not introduced in this PR, code that looks like a bug but isn't, pedantic nitpicks, issues already handled by linters, and issues with lint-ignore comments.

8. **Review comment format**: If any issues score ≥80, Claude posts a GitHub review comment using `gh`. Each issue includes a description, the CLAUDE.md rule it violates (if applicable), and a direct link to the code using full SHA and line range:
   ```
   https://github.com/owner/repo/blob/[full-sha]/path/file.ext#L[start]-L[end]
   ```

9. **Adjusting the threshold**: Edit the command file at `commands/code-review.md` and change the `80` value. Lower threshold = more issues reported, higher = stricter filtering.

10. **Adding custom agents**: Extend `commands/code-review.md` to add security-focused agents, performance analysis agents, accessibility agents, or documentation quality checks.

11. **Integration patterns**: Run on all PRs as part of the development workflow, or configure as part of CI/CD triggered on PR creation/update. The skip-if-reviewed logic prevents duplicate comments on reruns.

### Verification
- [ ] `gh` is installed and authenticated before running
- [ ] `/code-review` launches 4 parallel agents (visible in Claude output)
- [ ] Issues below confidence 80 do not appear in the final comment
- [ ] Code links use full SHA (not abbreviated) and `#L` notation
- [ ] No review comment is posted when all issues score below 80

## Cortex Logging
- collection: procedures
- tags: claude-code-plugins, training, code-review, confidence-scoring, pull-request, github, v2

## Enforcement Loop
- WHERE: GitHub repositories with Claude Code integration
- WHEN: Before merging any non-trivial pull request
- HOW: Run `/code-review` on PR branch; trust confidence filter; iterate on CLAUDE.md based on patterns found
- CONNECT: CCP-001 (feature-dev), CCP-003 (pr-review-toolkit), CCP-007 (claude-md-management)
