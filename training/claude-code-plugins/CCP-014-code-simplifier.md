---
type: procedure
created: 2026-03-20
status: active
slug: ccp-014-code-simplifier
tags: [procedure, nexus]
---

# CCP-014 — Autonomous Code Simplification Agent

## Problema
Code that works correctly often has unnecessary complexity — excessive nesting, redundant abstractions, clever one-liners that are hard to maintain, inconsistent patterns. After implementing a feature, developers rarely take time to simplify. The result is growing technical debt and declining readability.

## Procedura

### Prerequisites
- Claude Code installed
- Install: `/plugin install code-simplifier@claude-plugins-official`
- Agent: `code-simplifier` (model: Opus) — activated contextually or explicitly

### Steps

1. **Trigger the agent** explicitly:
   ```
   "Simplify this code"
   "Make this clearer"
   "Refine this implementation"
   ```
   Or use it as part of the PR review toolkit (CCP-003) workflow — it's included as one of the 6 specialized agents.

2. **The agent operates autonomously and proactively**: It refines code immediately after it's written or modified without requiring explicit requests, when active in the session.

3. **Core constraint — functionality preservation**: The agent never changes what the code does. Only how it does it. All original features, outputs, and behaviors must remain intact after simplification.

4. **Focus scope**: By default, the agent focuses on recently modified or touched code in the current session. To review a broader scope: "Simplify all code in `src/auth/`".

5. **Simplification dimensions applied**:

   - **Reduce nesting**: Flatten deeply nested conditionals using early returns or guard clauses.

   - **Eliminate redundancy**: Remove duplicate code, extract shared logic, consolidate related operations.

   - **Improve naming**: Rename variables and functions to clearly express intent.

   - **Remove unnecessary complexity**: Delete over-engineered abstractions that add indirection without benefit.

   - **Improve readability**: Choose explicit code over overly compact code. Expand multi-step operations that are compressed into unreadable one-liners.

   - **Consolidate related logic**: Group logically related operations together.

   - **Remove obvious comments**: Delete comments that describe what the code obviously does — keep only comments that explain *why*.

6. **Patterns the agent avoids**:
   - Nested ternary operators → prefer switch statements or if/else chains
   - Overly dense one-liners → prefer explicit multi-line expressions
   - Combining too many concerns into one function
   - Removing helpful abstractions that improve code organization
   - Prioritizing "fewer lines" over readability

7. **Project standards application**: The agent reads CLAUDE.md for project-specific coding standards and applies them:
   - ES modules with proper import sorting and extensions
   - `function` keyword preference over arrow functions
   - Explicit return type annotations for top-level functions
   - Proper React component patterns
   - Error handling patterns

8. **Recommended timing in workflow**:
   - After writing initial implementation and getting it working
   - After passing code review (as final polish step)
   - When code works but feels complex or inconsistent
   - Do NOT run before tests pass — functionality must be verified first

9. **Output**: The agent documents only significant changes that affect understanding. Minor formatting changes are applied silently. Structural changes are noted briefly.

### Verification
- [ ] Agent triggers on "simplify this code" / "make this clearer"
- [ ] Simplified code produces identical outputs to the original (all tests pass)
- [ ] Nested ternaries are replaced with if/else chains after simplification
- [ ] Agent focuses on recently modified code by default (not entire codebase)
- [ ] CLAUDE.md conventions are applied during simplification

## Cortex Logging
- collection: procedures
- tags: claude-code-plugins, training, code-simplifier, refactoring, code-quality, readability, v2

## Enforcement Loop
- WHERE: After implementing features or after passing code review
- WHEN: Code works but has unnecessary complexity or inconsistent patterns
- HOW: Ask "simplify this code" or include in pre-PR review sequence; always run tests after simplification to verify functionality
- CONNECT: CCP-003 (pr-review-toolkit includes code-simplifier), CCP-001 (feature-dev Phase 6)
