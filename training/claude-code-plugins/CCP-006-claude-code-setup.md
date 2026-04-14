---
type: procedure
created: 2026-03-20
status: active
slug: ccp-006-claude-code-setup
tags: [procedure, nexus]
---

# CCP-006 — Codebase Analysis and Automation Recommendations

## Problema
Developers setting up Claude Code on an existing project don't know which automations will provide the most value for their specific codebase. Generic recommendations waste time on irrelevant setup; missing high-value automations leaves productivity gains on the table.

## Procedura

### Prerequisites
- Claude Code installed and running in an existing project directory
- Install: `/plugin install claude-code-setup@claude-plugins-official`
- Skill triggers on: "recommend automations for this project", "help me set up Claude Code", "what hooks should I use?"

### Steps

1. **Trigger the skill naturally** with any of these phrases:
   ```
   "recommend automations for this project"
   "help me set up Claude Code"
   "what hooks should I use?"
   "what skills would be useful here?"
   ```

2. **The skill is read-only**: It analyzes your codebase but makes no changes. All recommendations must be implemented by you or by subsequent Claude Code instructions.

3. **Analysis scope** — Claude scans the codebase and examines:
   - Language and framework detection (TypeScript, Python, Go, Rust, etc.)
   - Build tools and test runners in use
   - Existing CI/CD configuration
   - File structure and patterns
   - Dependencies and external services

4. **MCP Server recommendations** — top 1-2 based on codebase:
   - `context7` for documentation lookup (especially for heavily documented libraries)
   - `playwright` for frontend projects with UI testing needs
   - Database MCPs for projects with database layers
   - Service-specific MCPs based on detected integrations

5. **Skills recommendations** — top 1-2 contextual skills:
   - Plan agent for complex feature work
   - `frontend-design` for projects with UI components
   - Domain-specific skills matching the project type

6. **Hooks recommendations** — top 1-2 automation hooks:
   - Auto-format on file save (matches detected formatter: prettier, black, rustfmt, etc.)
   - Auto-lint after edits (ESLint, pylint, clippy, etc.)
   - Block sensitive file patterns (`.env`, credential files)
   - Test runner hooks for TDD workflows

7. **Subagent recommendations**:
   - Security reviewer for projects handling auth or payments
   - Performance analyzer for performance-sensitive code
   - Accessibility checker for frontend projects

8. **Slash command recommendations**:
   - `/test` — run tests with appropriate arguments for the test framework detected
   - `/pr-review` — tailored to project's review conventions
   - `/explain` — explain complex parts of the specific codebase

9. **Output format**: Recommendations are presented as a prioritized list per category (top 1-2 per category), with rationale tied to specific codebase characteristics observed, and example configurations ready to copy.

10. **After receiving recommendations**: Use `plugin-dev` (CCP-004) to implement recommended hooks and skills, or `hookify` (CCP-008) for quick rule-based hook setup.

### Verification
- [ ] Skill triggers on "recommend automations for this project"
- [ ] Output covers at least MCP, skills, hooks, subagents, and slash commands categories
- [ ] Recommendations are specific to detected codebase characteristics (not generic)
- [ ] Skill makes no changes to any files
- [ ] Each recommendation includes rationale and example configuration

## Cortex Logging
- collection: procedures
- tags: claude-code-plugins, training, claude-code-setup, automation, project-setup, v2

## Enforcement Loop
- WHERE: Initial Claude Code setup on any project
- WHEN: Starting a new project with Claude Code, or auditing automation coverage on existing project
- HOW: Ask "recommend automations for this project" and implement the top-priority suggestions first
- CONNECT: CCP-004 (plugin-dev), CCP-007 (claude-md-management), CCP-008 (hookify)
