---
type: procedure
created: 2026-03-20
status: active
slug: readme
tags: [procedure, nexus]
---

# Claude Code Skills — Training Domain

**Domeniu**: `claude-code-skills`
**Creat**: 2026-03-20
**Scope**: Claude Code agent skills from mattpocock/skills — planning, TDD, architecture, tooling, knowledge management
**Sursa**: github.com/mattpocock/skills (MIT License, Matt Pocock 2026)
**Proceduri**: 16 (CCS-001 → CCS-016)

## Sub-domenii

### Planning & Design (CCS-001 → CCS-006)
PRD creation, plan generation, issue breakdown, interviewing, interface design, refactoring plans

### Development (CCS-007 → CCS-010)
TDD workflow, bug triage, architecture improvement, test migration

### Tooling & Setup (CCS-011 → CCS-012)
Pre-commit hooks, git guardrails

### Writing & Knowledge (CCS-013 → CCS-016)
Skill creation, article editing, DDD glossary, Obsidian vault management

## Key Philosophy
- Vertical slices (tracer bullets) over horizontal layers
- Deep modules (small interface, large implementation)
- Behavior-based testing (mock only at system boundaries)
- Durable descriptions (modules/behaviors, not file paths)

## Index

| Code | Title | Trigger |
|------|-------|---------|
| CCS-001 | Write a PRD | "write a PRD", "plan a new feature" |
| CCS-002 | PRD to Plan | "break this down", "tracer bullets" |
| CCS-003 | PRD to Issues | "convert to issues", "create tickets" |
| CCS-004 | Grill Me | "grill me", "stress-test this plan" |
| CCS-005 | Design an Interface | "design an interface", "design it twice" |
| CCS-006 | Request Refactor Plan | "plan a refactor", "refactoring RFC" |
| CCS-007 | TDD | "red-green-refactor", "test-first", "TDD" |
| CCS-008 | Triage Issue | "bug", "triage", "investigate this issue" |
| CCS-009 | Improve Codebase Architecture | "improve architecture", "make more testable" |
| CCS-010 | Migrate to Shoehorn | "shoehorn", "replace `as` in tests" |
| CCS-011 | Setup Pre-Commit | "add pre-commit hooks", "set up Husky" |
| CCS-012 | Git Guardrails | "add git guardrails", "block git push" |
| CCS-013 | Write a Skill | "create a skill", "write a skill" |
| CCS-014 | Edit Article | "edit this article", "revise my draft" |
| CCS-015 | Ubiquitous Language | "DDD", "domain model", "build a glossary" |
| CCS-016 | Obsidian Vault | "find a note", "add to the vault" |
