# PromptForge Pattern Index — P1 to P81

**Updated**: 2026-04-09 | **Sessions**: 1-11 | **Total**: 81 patterns (24 original + 45 Sessions 6-10 + 12 D3 Research)
**Source**: Training Sessions 1-5 (P1-P24), Sessions 6-10 (P25-P69), D3 Research Scan April 2026 (P70-P81)

---

## Sessions 1-5 (Original — P1-P24)

| ID | Name | Domain | Session |
|----|------|--------|---------|
| P1 | Agentic 8-Point System Prompt | Agent architecture | S1 |
| P2 | Socratic Constraint-First | Education/Analysis | S1 |
| P3 | Tiered Diagnostic Loop | Debugging/Analysis | S1 |
| P4 | Audience-Anchored Tutor | Education | S1 |
| P5 | 4-Phase Temporal Self-Improvement | Agent evolution | S1 |
| P6 | Permission Framing | All (+10-13 pts D4) | S2 |
| P7 | Classification Gate | All (Step 1 universal) | S2 |
| P8 | Negative Space | All (what NOT to do) | S2 |
| P9 | Domain-Specific Grounding | Agent prompts | S2 |
| P10 | CoT Root Cause (R1) | Debugging | S2 | **UPDATED 2026-04**: Default zero-shot CoT for reasoning-tuned models (Phi-4, Qwen3). Few-shot CoT can hurt (0.67→0.11 GSM8K). Source: arXiv 2604.07035 |
| P11 | ToT Strategic Decision (R2) | Strategy | S2 |
| P12 | Hypothesis-Evidence-Conclusion (R3) | Research | S2 |
| P13 | Agentic ReAct Pattern | Tool-use agents | S3 |
| P14 | ONE-Tool-Per-Iteration | Agentic safety | S3 |
| P15 | Verify-Before-Deliver (CoVe) | All quality gates | S4 | **UPDATED 2026-04**: Add checkpoint verification at 25-30% reasoning length for reasoning models. Detection-Extraction Gap: 52-88% of CoT is post-commitment. Source: arXiv 2604.06613 |
| P16 | Branch-Then-Prune (ToT) | Complex decisions | S4 | **UPDATED 2026-04**: For hierarchical problems, prefer CLoT (P72) over flat ToT. Use P16 for flat parallel alternatives only. Source: arXiv 2604.06805 |
| P17 | Stress-Test Assumptions (Self-Refine) | Analysis/Audit | S4 |
| P18 | Plan-Before-Solve | Complex multi-step | S4 |
| P19 | Compound Risk Detection | Security/Finance | S4 |
| P20 | Stable/Dynamic Split | Production/Caching | S5 | **UPDATED 2026-04**: Now 3-tier: Static (system prompt, policies, tools) / Semi-static (compaction summaries, key memory) / Dynamic (current turn, tool outputs). Source: Anthropic Context Engineering, April 2026 |
| P21 | Ranking Criteria Injection | Extraction/Lists | S5 |
| P22 | Confidence Scoring Rules | All scored outputs | S5 |
| P23 | Negative Examples as Constraints | All constraint tasks | S5 |
| P24 | Schema-First Output | Structured output | S5 | **UPDATED 2026-04**: Format Tax confirmed. For open-weight models: use P71 two-pass decoupling. For frontier models (GPT-5, Claude 4.x): P24 remains valid (minimal format tax). Source: arXiv 2604.03616 |

---

## Session 6 — Agentic System Prompts (P25-P35)

| ID | Name | Found In | NexusOS Status |
|----|------|----------|----------------|
| P25 | Anti-Affirmation Constraint | GPT-5, Cline, Claude Code | PARTIAL |
| P26 | Tiered Autonomy Declaration (Execute/Propose/Gate) | NexusOS, Clawdbot, Windsurf | STRONG (ours) | **UPDATED 2026-04**: Extend with Brain/Hands/Session (P76). Autonomy tiers reference context layer: Execute=session log, Propose=brain approval, Gate=human + session review. Source: Anthropic Managed Agents, April 8 2026 |
| P27 | Ephemeral Memory Filtering | GPT-5, Windsurf, Claude Code | GAP |
| P28 | Single Action Per Iteration | Manus, Windsurf, Claude Code | IMPLICIT |
| P29 | Identity Anchor Against Override | CLAUDE.md, Clawdbot, GPT-5 | PARTIAL |
| P30 | Proactive State Registration | NexusOS sub-agents, Manus | STRONG sub-agents |
| P31 | No-Estimate Policy | Claude Code, GPT-5, Richard | GAP |
| P32 | Implicit Context Relatedness Check | ChatGPT, GPT-4o, Windsurf | MISSING |
| P33 | Conciseness as Hard Constraint | Windsurf, Bolt, Manus, Perplexity | MISSING |
| P34 | Scope Immutability Under Spec | Claude Code, BUILDER, FIXER | STRONG sub-agents |
| P35 | Structural XML Sectioning | Windsurf, v0, Claude Code | PARTIAL | **UPDATED 2026-04**: Claude 4.x explicitly recommends nested hierarchy + consistent tag names. AP-3 (decoration) fully confirmed as anti-pattern. Source: Anthropic Claude 4.x Best Practices, April 2026 |

---

## Session 7 — Marketing/Business/SEO (P36-P47)

| ID | Name | Domain | Boost |
|----|------|--------|-------|
| P36 | Awareness-Stage Segmentation | Marketing/Copy | +10 pts |
| P37 | Multi-Framework Copywriting Stack (AIDA+PAS+BAB) | Marketing/Copy | +9 pts |
| P38 | Content Repurposing Matrix | Content | +12 pts |
| P39 | Role-with-Metric Injection | All marketing | +9 pts |
| P40 | Comparative Version Output | Copy/Ads | +8 pts |
| P41 | Business Framework Sequencer (Blue Ocean, MECE, Porter) | Strategy | +11 pts |
| P42 | Constraint-by-Platform | Ads/SEO/Social | +10 pts |
| P43 | Emotional Driver Matrix | Copy/Brand | +10 pts |
| P44 | SEO Intent-Format Match | SEO | +10 pts |
| P45 | Persona-Voice Injection | Brand/Content | +12 pts |
| P46 | Objection Pre-Emption Stack | Sales/Landing | +11 pts |
| P47 | Decision Framework Sequencer | Business/Strategy | +9 pts |

---

## Session 8 — Coding/Technical (P48-P54)

| ID | Name | Domain | Key Mechanism |
|----|------|--------|---------------|
| P48 | Severity-Tiered Feedback (Critical/High/Medium/Low + action) | Code Review | 4-tier + action threshold |
| P49 | Spec-First, Code-Second (Specification Gate) | Code Generation | 5-element spec before code |
| P50 | Systematic Debug Protocol (RIHV) | Debugging | Reproduce-Isolate-Hypothesize-Verify |
| P51 | Progressive Complexity Scaffolding | Education/Onboarding | Foundation→Core→Edge→Production |
| P52 | Before/After Contrast Frame | Refactoring | Named-principle annotation per diff |
| P53 | Language Constraint Propagation | All coding | Per-language idiom constraint block |
| P54 | Output-Only Extraction (JSON Strict Mode) | Structured output | No-explanation + recency placement |

---

## Session 9 — Creative/Education (P55-P62)

| ID | Name | Domain | Key Mechanism |
|----|------|--------|---------------|
| P55 | Show-Don't-Tell Enforcer | Fiction/Narrative | Sensory scaffold + scene-end pivot gate |
| P56 | Voice Capture + Transfer Chain | Ghostwriting/Brand | 5-dimension voice fingerprint extraction |
| P57 | Story Engine (Want/Need Conflict) | Fiction | WANT vs NEED conflict → climax choice |
| P58 | Feynman Gap Detector (3-phase) | Education | Simplify→Gap Audit→Depth Fill |
| P59 | Spaced Retrieval Scaffold | Learning/Onboarding | Interval schedule + retrieval type per interval |
| P60 | Content Unbundling Chain | Content Repurposing | Long-form→5 threads+3 posts+2 emails+1 video |
| P61 | Pedagogical Scaffolding Ladder (Bloom's) | Education | 6 rungs with inter-rung reference constraint |
| P62 | Self-Consistency Synthesis | Research/Decisions | 5 paths→majority→fault line if no consensus | **UPDATED 2026-04**: Self-consistency gains are model-specific heuristics. Use different temperature settings, not just phrasings. Cross-model transfer unreliable. Source: arXiv 2603.28038 |

---

## Session 10 — Design/Persona/Image (P63-P69)

| ID | Name | Domain | Key Mechanism |
|----|------|--------|---------------|
| P63 | Component Specification Matrix | UI/Design | Variant×Size×State exhaustive matrix |
| P64 | Library/Framework Mandate | Web Design/Code | Non-negotiable stack preamble |
| P65 | Persona Activation Stack (4-layer) | Persona/Role | Role→Behavioral Axiom→Output Contract→Calibration |
| P66 | Visual Quality Token Set | Image Generation | Model-specific quality/style/composition tokens | **UPDATED 2026-04**: MJ V7 Omni Reference (--sref+--cref simultaneous). Short dense phrases > verbose. V8 Alpha maturing. Structure: [Subject 3-5w] + [descriptors] + [mood] + [--sref] + [--cref] + [params]. Source: MJ V7 guides, April 2026 |
| P67 | Design Token Parameterization | Design Systems | XML design token block (colors, spacing, type) |
| P68 | Negative Image Scaffolding | Image Gen + UX | Systematic negative-space for visual outputs |
| P69 | Persona-to-Agent Promotion (PAP) | Meta/Agent Arch | 6-delta transformation from persona→SOUL |

---

## D3 Research Scan April 2026 (P70-P81)

| ID | Name | Domain | Key Mechanism | Source |
|----|------|--------|---------------|--------|
| P70 | Context Engineering Compaction Protocol | Agentic/Long-running | Compaction trigger threshold + recall-first memory management for agents >20 turns or >100K context | Anthropic "Effective Context Engineering", April 2026 |
| P71 | Reasoning-Formatting Decoupling (Two-Pass) | Structured Output | Separate reasoning (Pass 1) from formatting (Pass 2) to avoid Format Tax. Recovers 80-95% accuracy on open-weight models | arXiv 2604.03616 |
| P72 | Cognitive Loop of Thought (CLoT) | Math/Logic Reasoning | Hierarchical sub-problem decomposition + backward verification gates between levels. Prevents error propagation vs linear CoT | arXiv 2604.06805 |
| P73 | Adaptive Prompt Structure Factorization (aPSF) | Prompt Optimization | Decompose prompt into semantic factors, optimize each independently via interventional scoring. 45-87% token cost reduction | arXiv 2604.06699 |
| P74 | Sufficiency-Gated Early Exit (DTSR) | Reasoning Models (o3, Claude 4, Qwen3) | Prompt model to self-assess reasoning sufficiency. "Stop when sufficient" signals. 29-35% reasoning reduction, minimal performance loss | arXiv 2604.06787 |
| P75 | Evolved Role-Prompt Topology (HERA) | Multi-Agent Systems | Dual-axis adaptation: operational (WHAT) + behavioral (HOW). Topology as learnable parameter. +38.69% avg improvement over static roles | arXiv 2604.00901 |
| P76 | Brain/Hands/Session Architecture Prompt Design | Long-Running Agents | Brain (goals/policies) separated from Hands (tool execution) and Session (external event log). Brain queries session, doesn't dump history | Anthropic "Managed Agents", April 8 2026 |
| P77 | Utility-Guided Tool Call Orchestration | Tool-Heavy Agents | Explicit utility function: estimated gain, step cost, uncertainty, redundancy per action. Reduces unnecessary tool calls 40-60% vs free ReAct | arXiv 2603.19896 |
| P78 | Omni Reference Multimodal Prompting | Image Gen (MJ V7+) | Simultaneous --sref + --cref. Short dense phrase clusters + reference images. Verbose narrative deprecated for V7+ | MJ V7/V8 Guides, April 2026 |
| P79 | Instruction Motivation Framing | Claude 4.x Specific | [What to do] + [Why it matters]. Motivation activates value-based reasoning. More effective on Claude 4.x than prior generations | Anthropic "Claude 4.x Best Practices", April 2026 |
| P80 | Agentic Trace Compliance Specification | Agent QA/Production | Behavioral rules as extractable testable predicates. AgentPex-style automated trace validation. Catches process violations outcome-eval misses | arXiv 2603.23806 |
| P81 | Vibe Architecting Prevention Prompt | Code Gen/Architecture | 6 coupling types to declare explicitly: persistence, comms protocol, auth, deployment, observability, error handling. Prevents implicit LLM architecture decisions | arXiv 2604.04990 |

### Pattern Relationships (P70-P81)
- P70 extends P27 (Ephemeral Memory) with structured compaction protocol
- P71 complements P24 (Schema-First) with two-pass for open-weight models
- P72 supersedes P16 (ToT) for hierarchical reasoning tasks
- P75 extends P69 (PAP) with evolutionary adaptation mechanism
- P76 extends P26 (Tiered Autonomy) with infrastructure-level separation
- P77 extends P13 (ReAct) + P14 (ONE-Tool) with cost-benefit framework
- P78 updates P66 (Visual Quality Tokens) for MJ V7+ architecture
- P79 extends P9 (Domain-Specific Grounding) with motivation framing for Claude 4.x
- P80 extends P15 (CoVe) with specification-driven trace validation
- P81 extends P49 (Spec-First) with architecture-level constraint declaration

---

## Anti-Patterns Summary (31 total)

### Session 6 (Agentic): AP-1 to AP-6
- AP-1: Personality Without Constraint
- AP-2: Safety as User-Overridable
- AP-3: XML Tags as Decoration
- AP-4: Memory as Garbage Collection Problem
- AP-5: Confidence Without Calibration
- AP-6: Sycophantic Escalation (agreeing with wrong premises)

### Session 7 (Marketing): AP-M01 to AP-M08
- AP-M01: Vague Audience Placeholder
- AP-M02: Missing Platform Constraints
- AP-M03: Quantity Without Differentiation
- AP-M04 to AP-M08: (see session report)

### Session 8 (Coding): AP-C01 to AP-C06
- AP-C01: Implicit Spec ("Make it better")
- AP-C02: Severity Without Action Threshold
- AP-C03: Test Generation Without Failure Criteria
- AP-C04: Cross-Language Idiom Contamination
- AP-C05: Explanation Pollution in Structured Output
- AP-C06: Recursive Search Without Convergence

### Session 9 (Creative): AP-CR01 to AP-CR07
- AP-CR01: Context-Free Creative
- AP-CR02: Socratic in Monologue
- AP-CR03: Bloom Without Inter-Rung Connection
- AP-CR04: Voice Imitation by Vibe
- AP-CR05: Generated Knowledge Without Grounding
- AP-CR06: Spaced Rep Without Retrieval Type
- AP-CR07: Unbundling Without Native Hook Rewrite

### Session 10 (Design): AP-D01 to AP-D07
- AP-D01: Aesthetic Aspirationalism
- AP-D02: Role Label Without Behavioral Axiom
- AP-D03: Framework Plurality Without Prioritization
- AP-D04: Persona Without Output Contract
- AP-D05: Negative Prompts in DALL-E Context
- AP-D06: Persona Simulator Without Out-of-Band Protocol
- AP-D07: Over-scoped Multi-Component Dump

### D3 Research April 2026: AP-R01 to AP-R06
- AP-R01: Monolithic Prompt Optimization (treat prompt as single unit to optimize. Use aPSF factor-level instead)
- AP-R02: Always-On Few-Shot CoT (breaks reasoning-tuned models. Test zero-shot first)
- AP-R03: Verbose Image Prompts (deprecated for MJ V7+. Use short dense phrases + reference images)
- AP-R04: Linear CoT for Hierarchical Tasks (use CLoT P72 with backward verification instead)
- AP-R05: Transparent CoT Monitoring (52-88% of CoT is post-commitment narration. Use trace compliance P80 instead)
- AP-R06: Free-Form ReAct for Production (use Utility-Guided P77 with explicit cost-benefit)

---

## Quick Reference: Highest-Impact Patterns

**Universal (any prompt)**: P6 Permission Framing, P7 Classification Gate, P25 Anti-Affirmation, P33 Conciseness, P79 Instruction Motivation Framing (Claude 4.x)
**Agent/SOUL**: P26 Tiered Autonomy, P29 Identity Anchor, P65 Persona Activation Stack, P69 PAP, P70 Context Engineering, P75 HERA Evolved Roles, P76 Brain/Hands/Session, P80 Trace Compliance
**Marketing**: P36 Awareness Segmentation, P42 Constraint-by-Platform, P45 Persona-Voice
**Coding**: P48 Severity-Tiered, P49 Spec-First, P53 Language Constraints, P54 Output-Only, P81 Vibe Architecting Prevention
**Reasoning**: P10 CoT (zero-shot first!), P72 CLoT (hierarchical), P74 Sufficiency-Gated Early Exit
**Tool-Use Agents**: P13 ReAct (baseline), P77 Utility-Guided Orchestration (production)
**Structured Output**: P24 Schema-First (frontier), P71 Two-Pass Decoupling (open-weight)
**Creative**: P55 Show-Don't-Tell, P56 Voice Capture, P62 Self-Consistency
**Design/Image**: P63 Component Matrix, P64 Library Mandate, P66 Visual Quality Tokens, P67 Design Tokens, P78 Omni Reference (MJ V7+)
**Education**: P58 Feynman Gap Detector, P59 Spaced Retrieval, P61 Bloom's Ladder
**Optimization**: P73 aPSF (automated factor-level optimization)
