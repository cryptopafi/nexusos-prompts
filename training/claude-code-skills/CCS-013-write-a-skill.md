---
type: procedure
created: 2026-03-20
status: active
slug: ccs-013-write-a-skill
tags: [procedure, nexus]
---

# CCS-013 — Write a Skill (Create New Claude Code Skills)

## Problema
Ad-hoc agent instructions scattered across system prompts, inline comments, or chat history are unreliable and hard to maintain. A skill is a structured, reusable capability unit that the agent discovers through a description and loads on demand. Without a proper skill authoring process, descriptions are vague (agent can't decide when to load the skill), SKILL.md files are too long (slow to load), and bundled resources are missing (scripts that should be deterministic get regenerated expensively each time).

## Procedura

### Prerequisites
- Clear understanding of the task/domain the skill will cover
- Target skill directory (in the skills registry)
- Understanding of what distinguishes trigger cases from non-trigger cases

### Steps

1. **Gather requirements.** Ask the user: What task or domain does the skill cover? What specific use cases should it handle? Does it need executable scripts (deterministic operations, validation, formatting) or just instructions? Any reference materials to include? Are there edge cases that are explicitly out of scope?

2. **Design the directory structure.** A skill lives in a named directory containing: `SKILL.md` (required, main instructions); `REFERENCE.md` (detailed docs, only if needed); `EXAMPLES.md` (usage examples, only if needed); `scripts/` directory with utility scripts (only for deterministic operations). Keep the structure flat and minimal.

3. **Write the description first.** The description is the ONLY thing the agent sees when deciding which skill to load — it is surfaced in the system prompt alongside all other installed skills. The description must: be max 1024 characters; be written in third person; have the first sentence describe what the skill does; have the second sentence start with "Use when [specific triggers]."

4. **Make the description specific enough to discriminate.** A bad description: "Helps with documents." A good description: "Extract text and tables from PDF files, fill forms, merge documents. Use when working with PDF files or when user mentions PDFs, forms, or document extraction." The agent needs enough information to distinguish this skill from all other installed skills.

5. **Write the SKILL.md content.** Keep it under 100 lines. Include: frontmatter with `name` and `description`; quick start (minimal working example); numbered workflows with checklists for complex tasks; links to REFERENCE.md for advanced features (do not inline reference-level content in SKILL.md).

6. **Apply progressive disclosure.** Keep SKILL.md to the essential workflow. Move detailed docs, schemas, and advanced features to REFERENCE.md. Move usage examples to EXAMPLES.md. This keeps the primary load fast and the agent focused on the core workflow without being overwhelmed by edge cases.

7. **Split into separate files when needed.** Split when: SKILL.md would exceed 100 lines; content has distinct domains (e.g., finance schema vs sales schema — different audiences); advanced features are rarely needed for the common case.

8. **Add utility scripts for deterministic operations.** Add scripts when: the operation is deterministic (validation, formatting, checking); the same code would be generated from scratch repeatedly (wasted tokens and risk of variation); errors need explicit handling. Scripts save tokens and improve reliability compared to generated code.

9. **Avoid time-sensitive information.** Do not hardcode URLs that may change, version numbers that will be outdated, or credentials. The skill must remain valid over time without maintenance.

10. **Use consistent terminology.** Define terms once and use them consistently throughout all files in the skill. Inconsistent vocabulary forces the agent to resolve ambiguity rather than executing.

11. **Include concrete examples.** Abstract instructions without examples force the agent to infer intent. At least one concrete example per major workflow step.

12. **Ensure references are one level deep.** SKILL.md references REFERENCE.md. REFERENCE.md does not reference a third file. Deeper nesting creates navigation burden and the agent may not load all files.

13. **Review against the checklist.** Before finalizing: description includes triggers ("Use when..."); SKILL.md under 100 lines; no time-sensitive information; consistent terminology throughout; concrete examples included; all references are one level deep.

14. **Review with user.** Present the draft skill and ask: Does this cover your use cases? Is anything missing or unclear? Should any section be more or less detailed? Iterate until the user is satisfied.

### Verification
- [ ] Description is under 1024 characters and includes "Use when [specific triggers]"
- [ ] SKILL.md is under 100 lines
- [ ] No time-sensitive information (URLs, versions, credentials)
- [ ] At least one concrete example per major workflow
- [ ] References are one level deep (no chained reference files)

## Cortex Logging
- collection: procedures
- tags: claude-code-skills, training, skill-authoring, agent-skills, claude-code, v2

## Enforcement Loop
- WHERE: When building new reusable agent capabilities
- WHEN: User says "create a skill", "write a skill", "build a new skill"
- HOW: Write the description first (it's the hardest part); keep SKILL.md under 100 lines; add scripts for deterministic operations
- CONNECT: All CCS procedures were authored using this process; CCS-012 (git-guardrails is an example of a skill with a bundled script)
