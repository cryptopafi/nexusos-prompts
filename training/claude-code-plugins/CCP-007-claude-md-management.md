---
type: procedure
created: 2026-03-20
status: active
slug: ccp-007-claude-md-management
tags: [procedure, nexus]
---

# CCP-007 — CLAUDE.md Audit and Improvement

## Problema
CLAUDE.md files drift out of sync with the actual codebase as code evolves. Stale or incomplete CLAUDE.md means Claude misses important project conventions, gives wrong advice, and fails to enforce actual team guidelines. Additionally, session learnings — corrections and preferences discovered during work — are lost unless captured.

## Procedura

### Prerequisites
- Claude Code installed with an existing CLAUDE.md file (or intent to create one)
- Install: `/plugin install claude-md-management@claude-plugins-official`
- Two tools: `claude-md-improver` skill (codebase audit) + `/revise-claude-md` command (session capture)

### Steps

1. **Understand the two distinct tools and when to use each**:

   | Tool | Purpose | When to Use |
   |------|---------|-------------|
   | `claude-md-improver` (skill) | Align CLAUDE.md with current codebase state | After codebase changes, periodic maintenance |
   | `/revise-claude-md` (command) | Capture learnings from current session | End of session when new patterns were discovered |

2. **Trigger the `claude-md-improver` skill** for codebase alignment:
   ```
   "audit my CLAUDE.md files"
   "check if my CLAUDE.md is up to date"
   ```

3. **CLAUDE.md audit process**: Claude reads all CLAUDE.md files in the project, scans current codebase structure and patterns, and identifies:
   - Outdated information (references to deprecated patterns, old file paths)
   - Missing conventions actually used in the codebase but not documented
   - Quality scores per section (completeness, accuracy, specificity)
   - Specific recommended additions or removals

4. **Recommended content for CLAUDE.md**:
   - Project architecture and key abstractions
   - Naming conventions (files, functions, variables, types)
   - Preferred patterns (e.g., "use function keyword, not arrow functions for top-level")
   - Error handling conventions
   - Test framework and test file locations
   - Build and run commands
   - Forbidden patterns or common anti-patterns to avoid
   - Integration points and external service conventions

5. **Run `/revise-claude-md` at end of sessions** where new patterns were discovered:
   ```
   /revise-claude-md
   ```
   Claude reviews the current session's conversation, identifies corrections you made, preferences expressed, and patterns that emerged, then proposes targeted additions to CLAUDE.md.

6. **Session capture targets**: The command looks for moments where you corrected Claude's behavior, said "always do X", "never do Y", expressed frustration about a repeated mistake, or discovered a pattern that should be permanent.

7. **Hierarchical CLAUDE.md**: CLAUDE.md files work at directory levels — root covers project-wide conventions, subdirectory CLAUDE.md files cover area-specific conventions. The skill audits all levels.

8. **Quality improvement loop**:
   - Run `claude-md-improver` → review proposed changes → apply relevant ones
   - After each `/revise-claude-md` → review proposals → accept what's accurate
   - Re-run code-review (CCP-002) after updates to verify improved review quality

9. **Anti-patterns to avoid in CLAUDE.md**:
   - Overly vague instructions ("write good code")
   - Instructions that duplicate what's already obvious
   - Instructions so specific they only apply once
   - Outdated API references or deprecated patterns left in place

10. **Frequency**: Run `claude-md-improver` after major refactors or when adding new frameworks. Run `/revise-claude-md` at the end of any session that produced corrections or revealed missing guidelines.

### Verification
- [ ] `claude-md-improver` skill triggers on "audit my CLAUDE.md files"
- [ ] Skill output includes quality scores per CLAUDE.md section
- [ ] `/revise-claude-md` identifies at least the session corrections made
- [ ] Proposed changes are specific and codebase-grounded (not generic)
- [ ] After applying changes, subsequent code reviews show improved guideline compliance

## Cortex Logging
- collection: procedures
- tags: claude-code-plugins, training, claude-md, project-memory, conventions, v2

## Enforcement Loop
- WHERE: All Claude Code projects with CLAUDE.md files
- WHEN: After major codebase changes, at session end when corrections were made
- HOW: Alternate between `claude-md-improver` for proactive audits and `/revise-claude-md` for reactive capture
- CONNECT: CCP-002 (code-review uses CLAUDE.md), CCP-006 (claude-code-setup)
