---
type: procedure
created: 2026-03-20
status: active
slug: ccp-012-frontend-design
tags: [procedure, nexus]
---

# CCP-012 — Frontend Design (Anti-AI-Slop Guidance)

## Problema
AI-generated frontend code defaults to generic, indistinguishable aesthetics — bland layouts, default colors, Bootstrap-style components that look like every other AI-generated UI. The result is technically correct but visually forgettable and unprofessional, undermining product quality.

## Procedura

### Prerequisites
- Claude Code installed
- Install: `/plugin install frontend-design@claude-plugins-official`
- Skill activates **automatically** for frontend work — no explicit trigger needed

### Steps

1. **The skill activates automatically** when working on frontend code. No manual invocation required. Just describe what you want:
   ```
   "Create a dashboard for a music streaming app"
   "Build a landing page for an AI security startup"
   "Design a settings panel with dark mode"
   ```

2. **Core principle**: Claude makes a clear, intentional aesthetic choice before implementing. Instead of "safe" generic defaults, it commits to a specific visual direction based on the product context.

3. **What distinguishes this approach**:
   - Bold, opinionated aesthetic choices (not neutral defaults)
   - Distinctive typography — specific font pairings, weights, and sizes that create hierarchy
   - Unique color palettes — not default blues and grays; colors that reinforce the product's identity
   - High-impact animations and micro-interactions where appropriate
   - Visual details that make the UI feel crafted, not assembled

4. **Context-aware implementation**: Claude reads the project context (tech stack, existing styles, CLAUDE.md conventions) before generating code. Output matches the existing codebase patterns.

5. **Production-ready code**: Output is production-grade — not a mockup or prototype. Includes proper accessibility attributes, responsive breakpoints, and clean component structure.

6. **Aesthetic direction examples by product context**:
   - Music streaming app: dark themes, gradient accents, fluid animations
   - AI security startup: high-contrast, monospace elements, technical density
   - SaaS dashboard: data density, information hierarchy, calm information architecture
   - Consumer app: warmth, personality, approachable interactions

7. **Reference**: The Frontend Aesthetics Cookbook (github.com/anthropics/claude-cookbooks) contains detailed guidance on prompting for high-quality frontend design — useful for understanding the principles behind this skill.

8. **For fine-tuning**: After the initial implementation, give specific feedback about the direction:
   - "Make it feel more minimal and editorial"
   - "Increase the contrast and make the typography bolder"
   - "Add more whitespace and reduce the color palette to two tones"

9. **Combining with playground**: Use CCP-011 (playground) with the design-playground template to explore and configure visual parameters interactively before implementation with this skill.

### Verification
- [ ] Skill activates automatically on frontend work requests (no explicit trigger needed)
- [ ] Output includes a defined aesthetic direction (not generic defaults)
- [ ] Generated code uses specific color values and typography (not CSS defaults)
- [ ] Code is production-ready with accessibility attributes
- [ ] Implementation matches the product context described

## Cortex Logging
- collection: procedures
- tags: claude-code-plugins, training, frontend-design, ui, aesthetics, anti-slop, v2

## Enforcement Loop
- WHERE: Any frontend development work in Claude Code
- WHEN: Building UI components, landing pages, dashboards, settings screens
- HOW: Plugin activates automatically; give contextual product descriptions rather than generic UI descriptions; provide direction feedback to refine aesthetic
- CONNECT: CCP-011 (playground for design exploration), CCP-028 (playwright for visual testing)
