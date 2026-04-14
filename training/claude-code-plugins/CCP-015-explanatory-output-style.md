---
type: procedure
created: 2026-03-20
status: active
slug: ccp-015-explanatory-output-style
tags: [procedure, nexus]
---

# CCP-015 — Explanatory Output Style (Educational Insights via SessionStart)

## Problema
Claude Code's default output style is task-focused — it implements and explains minimally. Developers learning a new codebase or technology miss opportunities to understand *why* specific patterns are used, what trade-offs were made, and how the code connects to broader architectural decisions. The deprecated "Explanatory" output style filled this gap but was removed as a built-in setting.

## Procedura

### Prerequisites
- Claude Code installed
- Install: `/plugin install explanatory-output-style@claude-plugins-official`
- Token cost warning: this plugin adds extra instructions and produces longer responses — install only if you're comfortable with the additional token usage

### Steps

1. **Install and the plugin activates automatically**: A SessionStart hook fires at the start of every Claude Code session, injecting additional context instructions that change Claude's output style for the entire session. No manual activation needed per session.

2. **What changes in Claude's behavior**: Claude provides brief educational insights before and after writing code, formatted as:
   ```
   `★ Insight ─────────────────────────────────────`
   [2-3 key educational points]
   `─────────────────────────────────────────────────`
   ```

3. **Insight focus areas** (always codebase-specific, never generic):
   - Specific implementation choices made for this codebase
   - Patterns and conventions observed in the existing code
   - Trade-offs and design decisions at the point of implementation
   - Codebase-specific details (not "here's how async/await works in general")

4. **What insights are NOT**: Generic programming concepts or language tutorials. The skill is calibrated to provide context about *this specific codebase* and *this specific decision* — not general education.

5. **Replacing the deprecated setting**: If you previously used the `outputStyle: "Explanatory"` setting in Claude Code settings JSON, this plugin provides identical behavior through a SessionStart hook, which is the new mechanism for distributing such output style customizations.

6. **SessionStart hook pattern**: This plugin demonstrates that SessionStart hooks are roughly equivalent to project-level CLAUDE.md files but with the advantage of being distributable through the plugin marketplace. For output styles involving non-coding tasks, subagents are more appropriate than SessionStart hooks.

7. **Managing the plugin**:
   - **Disable** (keep installed): Use Claude Code plugin disable UI — code stays on device
   - **Uninstall** (remove code): Use Claude Code plugin uninstall UI
   - **Customize**: Create a local copy of the plugin, modify the `hooks-handlers/` content, use `--plugin-dir` to load the local version

8. **Combine with learning-output-style**: The `learning-output-style` plugin (CCP-016) extends explanatory mode with interactive learning — Claude stops at meaningful decision points and asks you to write key sections of the code yourself.

### Verification
- [ ] Plugin installs without error
- [ ] At session start, insights format (`★ Insight ─...`) appears during first code implementation
- [ ] Insights reference specific codebase patterns (not generic concepts)
- [ ] Disabling plugin stops insights from appearing in new sessions
- [ ] Token usage increases compared to default output style (expected behavior)

## Cortex Logging
- collection: procedures
- tags: claude-code-plugins, training, output-style, educational, session-start, hooks, v2

## Enforcement Loop
- WHERE: Development sessions focused on learning a new codebase or technology
- WHEN: Onboarding to unfamiliar code, studying architectural patterns, learning by building
- HOW: Install plugin; insights appear automatically; use the insight blocks as teaching moments
- CONNECT: CCP-016 (learning-output-style for interactive mode), CCP-004 (plugin-dev SessionStart hook pattern)
