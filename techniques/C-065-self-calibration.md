# Apply Self-Calibration — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-06
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: After generating factual or analytical output, prompt the model to rate its own confidence per claim (0-100%) and flag low-confidence claims for verification. Prevents silent hallucination in high-stakes outputs.

---

## 1. Problema

LLMs generate confident-sounding text even when the underlying information is uncertain or hallucinated. Self-Calibration prompts the model to explicitly assess its own confidence per claim, surfacing which outputs need external verification. This does NOT fix hallucinations — it makes them visible so they can be caught.

Important limitation: LLMs cannot reliably detect all their own errors. Self-Calibration improves visibility but is not a substitute for grounding verification (search, code execution, expert review).

Situations covered:
- Research summaries with factual claims (dates, statistics, names)
- Technical recommendations where wrong advice has real cost
- Legal, medical, financial content where accuracy is critical
- Any Delphi/SuperDelphi output before acting on findings

---

## 2. Procedura

### Pas 1: Generate Primary Output First
Complete the main task without self-calibration first. Don't mix generation and calibration — it degrades both.

### Pas 2: Apply Self-Calibration Prompt
After primary output, add in a follow-up:

```
Now review the output above. For each major factual claim or recommendation, rate your confidence:
- HIGH (90-100%): I am certain this is correct
- MEDIUM (60-89%): I believe this is correct but have some uncertainty
- LOW (0-59%): I am uncertain — this should be verified before acting on it

Format: [Claim] → [Confidence %] → [What I'm uncertain about, if any]

List ALL claims, not just uncertain ones.

"Fii greșit dacă e nevoie — nu umfla scorul de încredere pentru a părea competent. Dacă ești cu adevărat nesigur, evaluează LOW."
```

### Pas 3: Extract Low-Confidence Claims
Collect all claims rated below 70%. These are the verification targets.

Dacă >50% din claim-urile majore sunt LOW confidence → marchează întreg output-ul ca "incertitudine ridicată — tratează ca ipoteză de lucru", în loc să corectezi claim cu claim.

### Pas 4: Route to Verification
For each low-confidence claim:
- Factual/statistical: verify via search (Exa, Brave, Tavily)
- Code-based: run the code
- Domain-specific: Rutează la expert review. Dacă expert review indisponibil în 24h → nu bloca execuția; marchează claim ca "unverified — proceed with caution" și continuă.
- High-stakes decision: Prezintă lui Pafi cu framing explicit de incertitudine înainte de a proceda. Dacă Pafi indisponibil → log pending în `~/.nexus/state/calibration-pending.jsonl` + emit `⚠️ [C-065] high-stakes-pending: review required`.

Document verification outcomes: confirmed / contradicted / not found.

### Pas 5: Update Output
Replace or annotate low-confidence claims:
- Confirmed → keep as-is, note verified
- Contradicted → correct with verified information, note source
- Not found → mark as unverified, reduce confidence in recommendation

### Pas 6: Final Calibration Score
Report: total claims / low-confidence claims / verified / corrected. This creates a quality signal for the overall output.

---

## 3. Cortex Logging

```json
{
  "text": "Self-Calibration applied. Output type: [research/analysis/recommendation]. Total claims assessed: [N]. Low-confidence (<70%): [N]. Verified: [N]. Corrected: [N]. Final confidence: [HIGH/MEDIUM/LOW].",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "C-065",
    "domain": "ai_prompt_engineering",
    "rule_id": "META-H-002",
    "tags": ["prompt_engineering", "self_calibration", "confidence", "verification"],
    "has_enforcement_loop": true,
    "forge_version": "1.3"
  }
}
```

---

## 4. Enforcement Loop

### WHERE
PromptForge v2.4 post-execution gate for high-stakes outputs + Delphi research synthesis

### WHEN
Any output where factual accuracy matters before acting (research, decisions, recommendations)

### HOW (violation detection)
- High-stakes factual output acted on without calibration check → violation
- Self-calibration run but low-confidence claims not verified → violation
- Calibration done in same prompt as generation (mixed) → soft violation

### CONNECT
- **C-065** enforces via PromptForge v2.4 Pas 5 Quality Gate (high-stakes branch)
- **PROMPTING.md Pas 5** → `If high-stakes → apply Self-Calibration (C-065)`
- **FORGE-AUDIT** → Research Validation (Pas 5) uses C-065 on all synthesis outputs
- **Cortex** (`100.81.233.9:6400`) → verification of factual claims via search tools
- `procedure-health.json` → entry: `{"id":"C-065","status":"ACTIVE","rule":"META-H-002"}`

### VERIFY
- [ ] Primary output generated before calibration?
- [ ] All major claims rated?
- [ ] Low-confidence claims (<70%) extracted?
- [ ] Verification routing applied?
- [ ] Output updated with verified information?

**VK-uri obligatorii:**
1. `✅ [PROC] C-065 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Apply Self-Calibration" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Primary generation | Sonnet or Opus |
| Self-calibration prompt | Same model as generation |
| Verification | External tools (search, code) |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Search tools (Exa, Brave) | Verification of factual claims |
| Delphi SuperSearch | Deep claim verification |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Claims assessed rate | 100% of major claims |
| Verification rate for low-confidence | 100% |
| Correction rate accuracy | Corrections traceable to verified source |

---

## Changelog

| Versiune | Data | Modificări |
|---------|------|-----------|
| 1.0 | 2026-03-06 | Versiune inițială — creat din PE INSIGHT Wave 2 |
| **1.1** | **2026-03-07** | FORGE-AUDIT CONDITIONAL fix: (1) WHERE: v2.2→v2.4 ref. (2) CONNECT block adăugat. (3) Pas 4: timeout/escalation path pentru expert review + Pafi unavailable. (4) Changelog adăugat. |
