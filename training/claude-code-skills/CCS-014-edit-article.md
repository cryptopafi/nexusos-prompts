---
type: procedure
created: 2026-03-20
status: active
slug: ccs-014-edit-article
tags: [procedure, nexus]
---

# CCS-014 — Edit Article (DAG-Based Section Restructuring)

## Problema
Articles submitted for editing often have sections in the wrong order — a concept is explained before its prerequisites are introduced, or a conclusion is positioned before the reasoning that justifies it. Editing prose without first fixing the structural order produces polished sentences carrying broken logic. A DAG (directed acyclic graph) analysis of information dependencies ensures sections are ordered so that each piece of information builds on what the reader already knows.

## Procedura

### Prerequisites
- Article draft available (in conversation or as a file)
- Article has identifiable sections (via headings or natural topic breaks)
- User available to confirm section structure before rewriting begins

### Steps

1. **Divide the article into sections.** Read the full article. Identify sections based on headings (`#`, `##`, `###`) or, if headings are absent, on natural topic breaks where the subject shifts. List each section by its title or a brief descriptive label.

2. **Identify the main point of each section.** For each section, identify: What is the primary claim or information this section conveys? What does the reader know after reading this section that they did not know before?

3. **Model information as a DAG.** Information is a directed acyclic graph — pieces of information can depend on other pieces. A section that explains a consequence depends on a section that explains the cause. A section that evaluates a trade-off depends on a section that defines the options. Map these dependencies explicitly.

4. **Check that the current order respects all dependencies.** Walk through the current section order and verify that no section is positioned before a section it depends on. Identify any violations — sections where the reader is asked to understand something before the prerequisite concept has been introduced.

5. **Propose a reordered structure if needed.** If the dependency analysis reveals ordering violations, propose a corrected order that satisfies all dependencies. Present the proposed structure to the user as a numbered list of section titles in the new order. Explain why any moves were made (what dependency was violated in the original order).

6. **Confirm the sections with the user.** Do not rewrite any prose until the user has confirmed the section structure. Section structure changes are high-impact and hard to reverse after prose has been rewritten. Get explicit approval.

7. **Rewrite each section in the confirmed order.** Work through sections one by one. For each section: rewrite to improve clarity, coherence, and flow; enforce maximum 240 characters per paragraph; eliminate jargon unless it has been defined earlier in the article; ensure transitions between paragraphs flow naturally.

8. **Apply the 240-character paragraph limit strictly.** Paragraphs longer than 240 characters hide logical structure. A long paragraph usually contains two ideas that should be separated, or a claim that needs to be split from its supporting evidence. When a paragraph exceeds 240 characters, find the natural break point and split it.

9. **Ensure each paragraph has one job.** Each paragraph should: introduce one idea, develop one idea, or conclude one idea. A paragraph that does more than one of these should be split. A paragraph that does none of these should be deleted.

10. **Preserve the author's voice.** Do not substitute your vocabulary for the author's unless their word choice is genuinely unclear or incorrect. The goal is clarity and structure, not homogenization. Ask before making vocabulary changes that feel significant.

11. **Check dependency satisfaction after rewriting.** After completing all sections, re-read the article linearly and verify: Does each section assume only knowledge introduced in prior sections? Are there any forward references (references to concepts not yet explained)? If so, either move the referenced section earlier or add a brief introduction of the concept at first use.

### Verification
- [ ] Sections divided and main points identified before any rewriting
- [ ] DAG analysis completed (dependency violations identified)
- [ ] Section structure confirmed by user before prose rewriting
- [ ] All paragraphs are 240 characters or fewer
- [ ] No section assumes knowledge not yet introduced by prior sections

## Cortex Logging
- collection: procedures
- tags: claude-code-skills, training, writing, article-editing, dag, information-architecture, v2

## Enforcement Loop
- WHERE: Article or documentation drafts submitted for editing
- WHEN: User says "edit this article", "revise my draft", "improve this post", or shares a long-form text for editing
- HOW: Always analyze structure before prose; always confirm section order before rewriting; enforce 240-char paragraph limit
- CONNECT: CCS-015 (ubiquitous-language for domain terminology consistency in technical articles)
