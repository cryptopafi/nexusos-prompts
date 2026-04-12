# CRITIC — Tool-Assisted Verification — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-06
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: After generating output with factual claims or code, prompt the model to critique its own output and identify verifiable claims, then use external tools (search, code execution, API calls) to verify those claims and revise. Combines self-critique with grounded external verification.

---

## 1. Problema

LLMs cannot reliably self-correct without external feedback — intrinsic self-correction can make outputs worse. CRITIC (Gou et al., 2023) solves this by pairing self-critique with tool use: the model identifies what CAN be verified externally, uses tools to check, and revises based on tool results — not based on pure self-reasoning. This is the grounded version of Self-Refine.

Situations covered:
- Research summaries with factual claims (verify via search)
- Generated code (verify by running it)
- Mathematical calculations (verify via calculator/code)
- Recommendations citing statistics or studies (verify via search)
- API usage examples (verify by checking official docs)

---

## 2. Procedura

### Pas 1: Generate Primary Output
Complete the task normally. Don't interrupt generation with verification.

### Pas 2: CRITIC Prompt — Identify Verifiable Claims
Follow up with:

```
Review the output above. Identify all claims that can be verified using external tools:
1. Factual claims → can verify via web search
2. Code snippets → can verify by running them
3. Statistical claims → can verify via search
4. API/library usage → can verify via docs search

For each verifiable claim, output:
CLAIM: [exact claim]
TYPE: [factual/code/statistical/api]
VERIFICATION QUERY: [what to search or run to check this]
```

### Pas 3: Execute Verification
For each claim from Step 2:
- Factual/statistical: run Exa or Brave search with the verification query
- Code: execute in sandbox (Bash tool)
- API/library: fetch official docs
- Mathematical: run python calculation

Record: CLAIM → TOOL USED → RESULT → VERDICT (confirmed/contradicted/not found)

### Pas 4: CRITIC Revision Prompt
Feed verification results back:

```
The following claims in your output were verified using external tools:
[CLAIM 1] → [VERDICT + source]
[CLAIM 2] → [VERDICT + source]
[CLAIM 3] → [VERDICT + source]

Revise the output to:
1. Correct any contradicted claims with the verified information
2. Add a source citation for confirmed claims
3. Mark unverified claims as "[UNVERIFIED]" for human review
4. Remove or soften recommendations based on contradicted evidence
```

### Pas 5: Final Output Review
Verify the revision incorporated the corrections. Check that contradicted claims are actually removed/fixed (not just acknowledged).

---

## 3. Cortex Logging

```json
{
  "text": "CRITIC + Tool verification applied. Output type: [research/code/recommendation]. Claims identified: [N]. Verified: [N]. Confirmed: [N]. Contradicted: [N]. Not found: [N]. Revisions made: [N].",
  "collection": "technical",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "C-068",
    "domain": "ai_prompt_engineering",
    "rule_id": "META-H-002",
    "tags": ["prompt_engineering", "critic", "tool_verify", "self_correction", "grounding"],
    "has_enforcement_loop": true,
    "forge_version": "1.3"
  }
}
```

---

## 4. Enforcement Loop

### WHERE
PromptForge v2.2 Phase 2 + Delphi research verification + code generation workflows

### WHEN
Any output with factual claims, code, or statistics before high-stakes use

### HOW (violation detection)
- Self-critique without tool execution → violation (this is Self-Refine, not CRITIC — fails to provide grounding)
- Contradicted claims not corrected in final output → violation
- Tool results ignored in revision → violation

### VERIFY
- [ ] Primary output generated before critique?
- [ ] Verifiable claims explicitly identified?
- [ ] External tools executed for each verifiable claim?
- [ ] Revision explicitly incorporates tool verdicts?
- [ ] Contradicted claims removed/corrected in final output?

**VK-uri obligatorii:**
1. `✅ [PROC] C-068 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "CRITIC — Tool-Assisted Verification" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Primary generation | Sonnet or Opus |
| CRITIC prompt | Same model |
| Tool execution | Bash, Exa, Brave, Delphi |
| Revision | Same model |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Exa / Brave search | Factual verification |
| Bash tool | Code execution |
| SuperDelphi | Deep claim verification |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Claims identified per output | All verifiable claims (not just obvious ones) |
| Tool execution rate | 100% of identified claims |
| Correction application rate | 100% of contradicted claims corrected |
