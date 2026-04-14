---
type: procedure
created: 2026-03-20
status: active
slug: ccp-005-skill-creator
tags: [procedure, nexus]
---

# CCP-005 — Skill Creator with Eval Benchmarking

## Problema
Creating Claude Code skills without a systematic process leads to skills that don't trigger when they should (undertriggering), trigger in wrong contexts (overtriggering), or provide instructions Claude can't reliably follow. Without eval-driven iteration, skill quality is subjective and hard to measure.

## Procedura

### Prerequisites
- Claude Code installed
- Install: `/plugin install skill-creator@claude-plugins-official`
- Skill triggers on: "create a skill from scratch", "update or optimize an existing skill", "run evals to test a skill", "benchmark skill performance with variance analysis"

### Steps

1. **Start by capturing intent** — Claude extracts from conversation or asks:
   - What should this skill enable Claude to do?
   - When should this skill trigger (what user phrases/contexts)?
   - What is the expected output format?
   - Should test cases be set up? (Skills with objectively verifiable outputs benefit from evals; subjective outputs often don't need them.)

2. **Interview and research phase**: Claude proactively asks about edge cases, input/output formats, example files, success criteria, and dependencies. Available MCPs are checked for research. This happens before writing test prompts.

3. **Write the SKILL.md** with these components:
   - `name`: Skill identifier
   - `description`: The primary triggering mechanism — include both what the skill does AND specific contexts for when to use it. Make descriptions slightly "pushy" to combat undertriggering: instead of "How to build X", write "How to build X. Make sure to use this skill whenever the user mentions X, Y, or Z, even if they don't explicitly ask."
   - Skill body: instructions for Claude when skill is active

4. **Skill anatomy**:
   ```
   skill-name/
   ├── SKILL.md (required — YAML frontmatter + markdown instructions)
   └── Bundled Resources (optional)
       ├── scripts/    — Executable code for deterministic/repetitive tasks
       ├── references/ — Docs loaded into context as needed
       └── assets/     — Files used in output (templates, icons, fonts)
   ```

5. **Progressive disclosure — 3 levels**:
   - Metadata (name + description): always in context (~100 words)
   - SKILL.md body: in context when skill triggers (keep under 500 lines)
   - Bundled resources: loaded as needed (unlimited)

6. **Create test prompts**: Write a set of prompts that should trigger the skill and produce verifiable output. For each prompt, define expected behavior or assertions.

7. **Run evals**: Claude runs the test prompts with the skill active, executes them in parallel via subagents if available, and collects results. While runs happen in background, Claude drafts quantitative eval criteria.

8. **Review results with `eval-viewer`**: Use the `eval-viewer/generate_review.py` script to display results for qualitative review. Claude presents both qualitative assessment and quantitative metrics.

9. **Iterate based on findings**: Rewrite the skill based on user evaluation feedback and any glaring quantitative failures. Key areas to improve: trigger phrases (too broad, too narrow), instruction clarity, output format, edge case handling.

10. **Optimize the description for triggering accuracy**: After the skill content is solid, run the skill description improver (separate script) to optimize when the skill activates. This is the final tuning step.

11. **Scale up**: Once satisfied with small test set, expand to larger evaluation set and re-run. This catches variance and edge cases missed in small runs.

12. **Communication style**: Skill creator adapts to user technical level. Explains jargon when uncertain (JSON, assertions, benchmarks). Default tone is accessible but substantive.

### Verification
- [ ] SKILL.md has both `name` and `description` in YAML frontmatter
- [ ] Description includes specific trigger phrases (not just topic)
- [ ] Skill body is under 500 lines; longer content is in `references/`
- [ ] At least 3 test prompts are created before running evals
- [ ] Eval loop runs at least twice (draft → evaluate → iterate)

## Cortex Logging
- collection: procedures
- tags: claude-code-plugins, training, skill-creator, evals, benchmarking, skill-development, v2

## Enforcement Loop
- WHERE: When creating new Claude Code skills for any plugin
- WHEN: New skill is needed, existing skill undertriggers or produces inconsistent output
- HOW: Install skill-creator; follow the draft → eval → iterate loop; never ship a skill without at least one eval run
- CONNECT: CCP-004 (plugin-dev skill-development skill), CCP-031 (example-plugin)
