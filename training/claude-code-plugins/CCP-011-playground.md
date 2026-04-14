---
type: procedure
created: 2026-03-20
status: active
slug: ccp-011-playground
tags: [procedure, nexus]
---

# CCP-011 — Interactive HTML Playgrounds with Templates

## Problema
When users need to explore large input spaces visually — design parameters, data queries, concept relationships, document review — plain text conversation is insufficient. There's no way to interactively adjust parameters and see live results without a visual interface. Generating one-off HTML tools from scratch is slow and inconsistent.

## Procedura

### Prerequisites
- Claude Code installed
- Install: `/plugin install playground@claude-plugins-official`
- Skill triggers when user asks for an interactive playground, explorer, or visual tool for a topic

### Steps

1. **Trigger the skill** by asking for an interactive or visual exploration tool:
   ```
   "Create an interactive playground for CSS grid layout"
   "Build a visual explorer for SQL query building"
   "Make a concept map for learning React hooks"
   "Create a document review tool with approve/reject workflow"
   ```

2. **What a playground is**: A self-contained HTML file with:
   - Interactive controls on one side (sliders, dropdowns, text inputs, checkboxes)
   - Live preview on the other side
   - A prompt output at the bottom with a copy button
   The user adjusts controls, sees live preview, then copies the generated prompt back into Claude.

3. **Four built-in templates** — choose based on use case:

   - **design-playground**: For visual design decisions — components, layouts, spacing, color, typography. Controls adjust visual parameters; preview shows the rendered result; output is a design specification prompt.

   - **data-explorer**: For data and query building — SQL, APIs, data pipelines, regex. Controls configure the query/pipeline; preview shows example output or structure; output is a structured data request prompt.

   - **concept-map**: For learning and exploration — concept maps, knowledge gaps, scope mapping. Controls add/connect concepts; preview is a visual knowledge graph; output is a learning or explanation prompt.

   - **document-critique**: For document review — suggestions with approve/reject/comment workflow. Controls show suggestions; preview shows document with changes applied; output is a revision instruction prompt.

4. **When to use playgrounds**: The input space is large, visual, or structural and hard to express as plain text. Examples: "I want a button that looks like X but I'm not sure what exact parameters" or "I want to build a SQL query but need to visualize the joins".

5. **Output structure**: Every playground produces a prompt at the bottom that captures the user's configured state. This prompt can be copied back into Claude for further work, making playgrounds a structured input-capture tool rather than just a visualization.

6. **Self-contained**: Each playground is a single HTML file — no dependencies, no server required. Can be opened directly in a browser and shared as a file attachment.

7. **Customization**: The skill generates the playground HTML; ask Claude to adjust the controls, add new parameters, change the preview rendering, or modify the output prompt format after initial generation.

### Verification
- [ ] Skill triggers on "create an interactive playground for [topic]"
- [ ] Output is a single self-contained HTML file (no external dependencies)
- [ ] Playground has interactive controls, live preview, and a copyable prompt output
- [ ] One of the 4 templates is used as the base when the topic matches
- [ ] Generated HTML opens and works correctly in a browser

## Cortex Logging
- collection: procedures
- tags: claude-code-plugins, training, playground, interactive, html, visual-tools, v2

## Enforcement Loop
- WHERE: Claude Code sessions where users need to explore large parameter spaces
- WHEN: User wants to configure something visually before expressing it as text
- HOW: Ask for "an interactive playground for [topic]"; pick the right template; copy the generated prompt back into Claude for follow-up work
- CONNECT: CCP-012 (frontend-design for implementing the resulting designs)
