---
type: procedure
created: 2026-03-20
status: active
slug: ccs-009-improve-codebase-architecture
tags: [procedure, nexus]
---

# CCS-009 — Improve Codebase Architecture (Architectural Friction Detection)

## Problema
Shallow, tightly-coupled modules are the primary driver of poor testability and AI-navigation difficulty in codebases. Pure functions extracted just for testability leave the real bugs hiding in how those functions are called. Tightly-coupled modules create integration risk at every seam. Without systematic friction detection followed by module-deepening refactors, codebases accumulate architectural debt that makes each new feature harder to implement correctly.

## Procedura

### Prerequisites
- Codebase accessible for organic exploration (not rigid heuristic scanning)
- `gh` CLI authenticated for RFC issue creation
- Ability to spawn parallel sub-agents (Agent tool / Task tool)
- Understanding of dependency categories (in-process, local-substitutable, remote-owned, true external)

### Steps

1. **Explore the codebase organically.** Navigate the codebase as an AI agent would — naturally, following understanding rather than rigid heuristics. Note where friction is experienced: Where does understanding one concept require bouncing between many small files? Where are modules so shallow that the interface is nearly as complex as the implementation? Where have pure functions been extracted just for testability but the real bugs hide in how they're called? Where do tightly-coupled modules create integration risk at the seams between them? Which parts are untested or hard to test?

2. **The friction you encounter IS the signal.** Do not look for abstract architectural patterns — look for where navigation and understanding is painful. That pain is the codebase telling you where the shallow modules are.

3. **Present deepening candidates.** For each candidate found, show: Cluster (which modules/concepts are involved); Why they're coupled (shared types, call patterns, co-ownership of a concept); Dependency category (in-process / local-substitutable / remote-owned / true external); Test impact (what existing tests would be replaced by boundary tests on the deepened module).

4. **Do NOT propose interfaces yet.** At the candidate presentation stage, only identify problems, not solutions. Ask the user: "Which of these would you like to explore?" Premature interface proposals close off exploration.

5. **Classify the dependency category.** In-process: pure computation, in-memory state, no I/O — always deepenable. Local-substitutable: dependencies with local test stand-ins (PGLite for Postgres, in-memory filesystem) — deepenable if stand-in exists. Remote but owned (Ports & Adapters): your own services across a network boundary — define a port at the module boundary with an HTTP adapter for production and an in-memory adapter for testing. True external (Mock): third-party services (Stripe, Twilio) — mock at the boundary with an injected port.

6. **Frame the problem space for the chosen candidate.** Before spawning design sub-agents, write a user-facing explanation: the constraints any new interface must satisfy; the dependencies it must rely on; a rough illustrative code sketch to make constraints concrete (this is not a proposal, just to ground the constraints). Show this to the user, then immediately proceed to parallel design. The user reads while agents work.

7. **Spawn 3+ sub-agents in parallel for interface design.** Give each agent a technical brief (coupling details, dependency category, what to hide) and a different design constraint: Agent 1: "Minimize the interface — aim for 1-3 entry points max"; Agent 2: "Maximize flexibility — support many use cases and extension"; Agent 3: "Optimize for the most common caller — make the default case trivial"; Agent 4 (if applicable): "Design around ports & adapters pattern for cross-boundary dependencies."

8. **Each sub-agent produces five artifacts.** Interface signature (types, methods, params); usage example showing how callers use it; what complexity it hides internally; dependency strategy (how dependencies are handled per the category classification); trade-offs of this approach.

9. **Present designs sequentially, then compare in prose.** Show each design fully before the next. Compare on depth, simplicity, implementation efficiency, and testability. Then give your own recommendation — be opinionated. The user wants a strong read, not a menu. If elements from different designs combine well, propose a hybrid.

10. **Apply "replace, don't layer" testing strategy.** Old unit tests on shallow modules are waste once boundary tests exist — delete them. Write new tests at the deepened module's interface boundary. Tests assert on observable outcomes through the public interface, not internal state.

11. **Create GitHub RFC issue.** After user picks an interface (or accepts your recommendation), create the RFC with `gh issue create` immediately without asking for review. Include: Problem (architectural friction description), Proposed Interface (chosen design with usage example and what it hides), Dependency Strategy (which category applies and how dependencies are handled), Testing Strategy (new boundary tests to write + old tests to delete + test environment needs), Implementation Recommendations (module responsibilities, what it hides, what it exposes, how callers migrate — durable, not coupled to file paths).

### Verification
- [ ] Exploration was organic (friction-driven), not heuristic-based
- [ ] Dependency category classified before proposing interfaces
- [ ] At least 3 parallel design agents spawned with different constraints
- [ ] Recommendation is opinionated (not just a menu of options)
- [ ] RFC includes explicit "tests to delete" (replace, don't layer)

## Cortex Logging
- collection: procedures
- tags: claude-code-skills, training, architecture, deep-modules, refactoring, testability, rfc, v2

## Enforcement Loop
- WHERE: Architectural review sessions or when a codebase feels painful to navigate
- WHEN: User says "improve architecture", "find refactoring opportunities", "make this more testable", or "AI-navigable"
- HOW: Explore organically first; classify dependencies; parallel design agents; opinionated recommendation
- CONNECT: CCS-005 (design-an-interface), CCS-006 (refactor planning), CCS-007 (TDD for the refactor)
