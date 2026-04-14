---
type: procedure
created: 2026-03-20
status: active
slug: ccp-001-feature-dev
tags: [procedure, nexus]
---

# CCP-001 — Feature Development with Parallel Agents

## Problema
Building new features without a structured process leads to poor codebase integration, missed requirements, and low-quality code. Developers jump straight into implementation without understanding existing patterns, resulting in inconsistent architecture and bugs that surface late.

## Procedura

### Prerequisites
- Claude Code installed and active in a git repository
- Existing codebase (workflow assumes existing code to learn from)
- Install: `/plugin install feature-dev@claude-plugins-official`

### Steps

1. **Invoke the command** with a feature description:
   ```
   /feature-dev Add user authentication with OAuth
   ```
   Or run `/feature-dev` alone to be guided interactively.

2. **Phase 1 — Discovery**: Claude clarifies the feature request. It asks what problem you're solving, identifies constraints, and confirms understanding before proceeding. Be specific upfront to reduce back-and-forth.

3. **Phase 2 — Codebase Exploration**: Claude launches 2-3 `code-explorer` agents in parallel. Each agent traces a different aspect — similar features, architecture patterns, UI conventions. Agents return entry points, execution flows, and a list of key files to read. Claude reads all identified files and presents a comprehensive summary.

4. **Phase 3 — Clarifying Questions**: Claude reviews findings and identifies all underspecified aspects: edge cases, error handling, integration points, backward compatibility, performance needs. All questions are presented as an organized list. **Claude waits for your answers before proceeding.** This phase prevents ambiguity before design begins.

5. **Phase 4 — Architecture Design**: Claude launches 2-3 `code-architect` agents simultaneously, each with a different focus:
   - *Minimal changes* — smallest footprint, maximum reuse
   - *Clean architecture* — maintainability and elegant abstractions
   - *Pragmatic balance* — speed plus quality

   Claude consolidates findings, states its recommendation with rationale, and **asks which approach you prefer** before moving forward.

6. **Phase 5 — Implementation**: Claude waits for your explicit approval, reads all relevant files identified in earlier phases, then implements following the chosen architecture and codebase conventions. Progress is tracked via todos throughout.

7. **Phase 6 — Quality Review**: Three `code-reviewer` agents run in parallel, each focused on a different dimension — simplicity/DRY/elegance, bugs/correctness, and conventions/abstractions. Issues are confidence-scored (0-100); only issues at 75+ are surfaced. Claude presents findings categorized by severity and **asks what you want to do**: fix now, fix later, or proceed as-is.

8. **Phase 7 — Summary**: Todos are marked complete. Claude summarizes what was built, key architectural decisions made, files modified, and suggested next steps.

9. **Manual agent invocation**: Any agent can be called independently without running the full workflow:
   ```
   "Launch code-explorer to trace how authentication works"
   "Launch code-architect to design the caching layer"
   "Launch code-reviewer to check my recent changes"
   ```

10. **Know when to use it**: Use for features touching multiple files, requiring architectural decisions, or with unclear requirements. Skip for single-line fixes, trivial changes, or urgent hotfixes.

11. **Confidence scoring** in code-reviewer: 0=false positive, 25=uncertain, 50=minor, 75=important, 100=critical. Default filter shows only issues ≥80 confidence to reduce noise.

12. **Troubleshooting**: If Phase 3 generates too many questions, provide more context in the initial request. If Phase 4 options overwhelm, trust the recommendation — it's based on codebase analysis.

### Verification
- [ ] `/feature-dev` command is available after plugin install
- [ ] Parallel agents launch in Phase 2 (multiple code-explorer instances)
- [ ] Claude waits for approval before Phase 5 (implementation)
- [ ] Phase 6 quality review surfaces issues with file:line references
- [ ] Phase 7 summary lists all modified files

## Cortex Logging
- collection: procedures
- tags: claude-code-plugins, training, feature-dev, parallel-agents, code-review, v2

## Enforcement Loop
- WHERE: Claude Code sessions where new features are being built
- WHEN: Feature request involves multiple files or architectural decisions
- HOW: Run `/feature-dev [description]` and follow all 7 phases without skipping
- CONNECT: CCP-002 (code-review), CCP-003 (pr-review-toolkit), CCP-004 (plugin-dev)
