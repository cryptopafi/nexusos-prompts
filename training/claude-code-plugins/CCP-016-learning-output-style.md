---
type: procedure
created: 2026-03-20
status: active
slug: ccp-016-learning-output-style
tags: [procedure, nexus]
---

# CCP-016 — Learning Output Style (Interactive Learning Mode)

## Problema
Passive observation of AI coding doesn't build skills. Watching Claude implement everything creates dependency without developing the developer's own understanding of business logic, architectural choices, and design patterns. The learning happens when developers make decisions and write key code themselves — not when they read Claude's output.

## Procedura

### Prerequisites
- Claude Code installed
- Install: `/plugin install learning-output-style@claude-plugins-official`
- Token cost and interactivity warning: this plugin adds extra instructions, produces longer responses, and interrupts implementation flow — install only if actively learning

### Steps

1. **Plugin activates automatically via SessionStart hook**: At the start of every session, additional context is injected that instructs Claude to combine two modes — interactive learning + educational explanations.

2. **Two combined behaviors**:

   **Learning Mode** (interactive): Claude identifies opportunities where you can write 5-10 lines of meaningful code. At decision points, Claude:
   - Stops instead of implementing automatically
   - Explains the trade-offs and context
   - Prepares the file and location for your contribution
   - Guides your implementation approach
   - Waits for you to write the code before continuing

   **Explanatory Mode** (insights): After writing code (yours or Claude's), educational insights appear in the star-border format:
   ```
   `★ Insight ─────────────────────────────────────`
   [2-3 key educational points]
   `─────────────────────────────────────────────────`
   ```

3. **When Claude requests your contribution** (learning mode intervention points):
   - Business logic with multiple valid approaches
   - Error handling strategies
   - Algorithm implementation choices
   - Data structure decisions
   - User experience decisions
   - Design patterns and architectural choices

4. **When Claude implements directly** (no interruption):
   - Boilerplate or repetitive code
   - Obvious implementations with no meaningful choices
   - Configuration or setup code
   - Simple CRUD operations

5. **Example interaction**:
   ```
   Claude: "I've set up the authentication middleware. The session timeout
   behavior is a security vs. UX trade-off — should sessions auto-extend
   on activity, or have a hard timeout?

   In auth/middleware.ts, implement handleSessionTimeout() to define this.

   Consider: auto-extending improves UX but may leave sessions open longer;
   hard timeouts are more secure but might frustrate active users."

   You: [Write 5-10 lines implementing your preferred approach]

   Claude: [Continues, then shows insight about the approach chosen]
   ```

6. **Difference from explanatory-output-style (CCP-015)**:
   - CCP-015: Adds educational insights, Claude still implements everything
   - CCP-016: Adds educational insights AND makes Claude pause for your contributions at meaningful decision points
   - CCP-016 includes all CCP-015 functionality

7. **Philosophy**: Learning by doing is more effective than passive observation. This plugin transforms the interaction from "watch and learn" to "build and understand."

8. **Managing the plugin**:
   - **Disable**: Use plugin disable UI — keeps code on device, stops hooks
   - **Uninstall**: Removes code entirely
   - **Customize**: Create local copy, modify hooks-handlers/, load with `--plugin-dir`

9. **Not appropriate for**: Production work under time pressure, urgent bug fixes, or any situation where interactive interruptions would be counterproductive. Enable for dedicated learning sessions.

### Verification
- [ ] Plugin activates at session start without manual invocation
- [ ] Claude stops at decision points and asks for your code contribution
- [ ] Insights appear in `★ Insight ─...` format after code is written
- [ ] Claude implements directly for boilerplate (no unnecessary interruptions)
- [ ] Disabling plugin restores default non-interactive behavior

## Cortex Logging
- collection: procedures
- tags: claude-code-plugins, training, learning-mode, interactive, session-start, hooks, v2

## Enforcement Loop
- WHERE: Dedicated learning sessions or onboarding to new codebases/technologies
- WHEN: Goal is skill development, not just task completion
- HOW: Install plugin; let it interrupt at decision points; write the requested 5-10 lines; use insights for reflection
- CONNECT: CCP-015 (explanatory-output-style for non-interactive mode), CCP-004 (SessionStart hook pattern)
