---
type: procedure
created: 2026-03-20
status: active
slug: ccs-005-design-an-interface
tags: [procedure, nexus]
---

# CCS-005 — Design an Interface (Multi-Design Parallel Exploration)

## Problema
A developer's first interface design is rarely the best. Without deliberate comparison of multiple radically different approaches, teams commit to the first design that works, missing simpler, more general, or more testable alternatives. The "Design It Twice" principle (from "A Philosophy of Software Design" by John Ousterhout) mandates generating multiple designs before selecting one.

## Procedura

### Prerequisites
- Clear description of the module to be designed (what problem it solves, who the callers are)
- Understanding of constraints (performance, compatibility, existing patterns)
- Ability to spawn parallel sub-agents (Agent tool / Task tool)

### Steps

1. **Gather requirements before designing.** Understand: What problem does this module solve? Who are the callers (other modules, external users, tests)? What are the key operations? Any constraints (performance, compatibility, existing patterns)? What should be hidden inside vs exposed in the interface? Ask: "What does this module need to do? Who will use it?"

2. **Identify what should be hidden.** The value of an interface is what it hides. Before generating designs, list the implementation details that callers should never need to know about — I/O format, internal data structures, concurrency handling, caching, validation logic, error normalization.

3. **Spawn 3+ sub-agents in parallel.** Use the Agent/Task tool to run multiple design explorations simultaneously. Assign each agent a radically different constraint to force divergent designs: Agent 1: "Minimize method count — aim for 1-3 methods max"; Agent 2: "Maximize flexibility — support many use cases and extension"; Agent 3: "Optimize for the most common case — make the default trivial"; Agent 4 (optional): "Take inspiration from [specific paradigm, e.g., builder pattern, functional pipeline, event-driven]".

4. **Enforce radical difference between agents.** Do not let sub-agents produce similar designs. If two agents converge on the same shape, reassign one with a more extreme constraint. The value of this exercise is in contrast, not consensus.

5. **Each sub-agent produces four artifacts.** Interface signature (types, methods, params); usage example (how callers actually use it in practice — not hypothetically); what the design hides internally (the implementation complexity the caller is spared); trade-offs of this approach (what it sacrifices to achieve its design goal).

6. **Present designs sequentially.** Show each design fully before moving to the next. Give the user time to absorb each approach before comparison. Do not present all designs in a wall of text.

7. **Compare designs on five axes.** Interface simplicity (fewer methods, simpler params = easier to learn and use correctly); general-purpose vs specialized (flexibility vs focus); implementation efficiency (does the shape allow efficient internals, or does it force awkward implementations?); depth (small interface hiding significant complexity = deep module — good; large interface with thin implementation = shallow module — avoid); ease of correct use vs ease of misuse.

8. **Use prose for comparison, not tables.** Tables flatten nuance. Discuss trade-offs in prose that highlights where designs diverge most and what that divergence costs or enables.

9. **Evaluate against deep module principle.** A deep module (from "A Philosophy of Software Design") has a small interface and large implementation. Ask of each design: Can I reduce the number of methods? Can I simplify the parameters? Can I hide more complexity inside?

10. **Synthesize the best elements.** After comparison, propose a synthesis. Often the best design combines insights from multiple options — the minimal surface of Design 1 with the flexibility hook of Design 3. Present a synthesized recommendation and explain why it's better than any single design in isolation.

11. **Ask for user choice.** Confirm: "Which design best fits your primary use case? Any elements from other designs worth incorporating?" Do not implement — this is purely about interface shape.

### Verification
- [ ] At least 3 radically different designs generated in parallel
- [ ] Each design includes usage examples (not just signatures)
- [ ] Designs compared in prose highlighting divergence, not just listed
- [ ] Deep module principle applied as evaluation criterion
- [ ] Synthesis proposed before asking for user choice

## Cortex Logging
- collection: procedures
- tags: claude-code-skills, training, interface-design, design-it-twice, deep-modules, architecture, v2

## Enforcement Loop
- WHERE: When designing a new module, API, or significant refactor of a module interface
- WHEN: User says "design an interface", "explore options", "compare module shapes", or "design it twice"
- HOW: Always run designs in parallel; never skip comparison; never implement during this step
- CONNECT: CCS-009 (improve-codebase-architecture uses same parallel design approach), CCS-007 (TDD — interface design precedes tests)
