---
type: procedure
created: 2026-03-20
status: active
slug: ccs-004-grill-me
tags: [procedure, nexus]
---

# CCS-004 — Grill Me (Decision Tree Interviewing)

## Problema
Design plans presented by developers often contain unexamined assumptions, unresolved dependencies, and branches of the decision tree that were never walked. When these gaps reach implementation, they cause mid-sprint pivots, architectural rewrites, or features that don't match what was actually needed. A structured, relentless interview process surfaces these gaps before any code is written.

## Procedura

### Prerequisites
- A plan, design, or idea from the user (can be rough or detailed)
- Codebase accessible for exploratory verification
- User available for extended back-and-forth questioning session

### Steps

1. **Receive the plan.** Let the user describe their plan or design in whatever form they have it. Do not start questioning until they have finished presenting their initial thinking.

2. **Explore the codebase to verify assertions.** Before formulating interview questions, check the codebase to verify any factual claims the user made. If a question can be answered by looking at existing code, look at the code instead of asking the user. This respects the user's time and grounds the interview in reality.

3. **Map the decision tree.** Mentally construct a tree of all decisions embedded in the plan. Each branch represents a design choice, and each branch has sub-branches that depend on how the parent is resolved. Identify the root decisions (most consequential, others depend on them) and the leaf decisions (specific implementation choices that depend on earlier decisions).

4. **Start with root decisions.** Begin the interview with the most fundamental, high-consequence decisions. Getting these right prevents wasted effort on sub-decisions that would change based on root choices. Work top-down through the tree.

5. **Provide recommended answers.** For every question asked, provide your own recommended answer with reasoning. This is not optional — recommended answers prevent the interview from feeling like an interrogation and give the user a concrete position to react to rather than generating an answer from scratch.

6. **Resolve dependencies between decisions one-by-one.** When a decision depends on an earlier unresolved decision, pause and resolve the dependency first before proceeding. Do not accumulate open questions — close each branch before opening new ones.

7. **Walk every branch of the tree.** Do not stop early because the core design seems clear. Edge cases, error states, rollback scenarios, scale limits, security boundaries, and operational concerns all represent branches. Walk them all.

8. **Probe for hidden assumptions.** Ask "What happens when X fails?", "What if Y doesn't exist?", "Who owns Z?", "How does this behave under load?". Assumptions that seem obvious to the designer are often the ones that break in production.

9. **Probe for scope creep risks.** Ask the user to articulate what is explicitly out of scope. Undrawn scope boundaries allow scope to expand silently during implementation.

10. **Synthesize shared understanding.** After all branches are resolved, summarize the decisions reached. Present it as a list of resolved decisions with outcomes. Confirm with the user that this accurately captures the shared understanding.

11. **Identify remaining open questions.** Any decisions that could not be resolved in the interview (require stakeholder input, external data, etc.) should be listed explicitly as open questions with owners assigned.

### Verification
- [ ] All root decisions resolved before leaf decisions
- [ ] Every recommendation includes reasoning, not just a choice
- [ ] Codebase checked to answer verifiable questions before asking user
- [ ] Error states and failure scenarios explicitly covered
- [ ] Summary of shared understanding confirmed by user

## Cortex Logging
- collection: procedures
- tags: claude-code-skills, training, interviewing, planning, decision-tree, design-review, v2

## Enforcement Loop
- WHERE: Before any significant design or implementation begins
- WHEN: User says "grill me", "stress-test this plan", "what am I missing", or presents a design for review
- HOW: Walk the full decision tree; provide recommended answers for every question; resolve dependencies in order
- CONNECT: CCS-001 (write-a-prd uses this technique), CCS-005 (design-an-interface for module design), CCS-006 (refactor planning)
