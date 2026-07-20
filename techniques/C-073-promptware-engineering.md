---
id: C-073
title: Promptware Engineering
domain: ai-prompt-engineering
version: 1.0
forge_audit: PASS
created: 2026-03-06
source: arxiv:2503.02400v2 (Promptware Engineering, 2025) | EMNLP 2025 APO Survey (arxiv:2502.16923)
---

## §0 — Objective

Treat prompts as **software artifacts** — applying software engineering disciplines (versioning, testing, CI/CD, lifecycle management) to prompt development. For systems where prompts ARE the primary programming interface.

## §1 — Theory

**Promptware** (arxiv:2503.02400v2, 2025) = a new software paradigm where:
- LLMs are increasingly integrated into software applications
- Prompts serve as the primary "programming" interface for system behavior
- Prompts require the same rigor as code: versioning, testing, regression prevention, deployment pipelines

**Lifecycle** parallels software engineering:
```
REQUIREMENTS → DESIGN → IMPLEMENT → TEST → DEPLOY → MONITOR → ITERATE
(intent)       (structure) (prompt)  (eval) (A/B)    (metrics)  (refine)
```

**Key shift**: From "prompts as text" → "prompts as versioned, tested, deployable artifacts"

**EMNLP 2025 APO Survey** (arxiv:2502.16923): systematic coverage of automatic prompt optimization (APE, OPRO, AutoPrompt) — these are the automated layer on top of promptware engineering.

## §2 — When to Apply

**USE for**:
- Production systems with multiple prompt variants
- Teams collaborating on prompt development
- Systems where prompt quality is business-critical
- Multi-model deployments (different prompts per model)

**SKIP for**:
- Ad-hoc single-use prompts
- Conversational interactions
- Experimental/research prompts with short lifecycle

## §3 — Procedure

### Step 1: Establish prompt as versioned artifact
```
prompts/
├── system/
│   ├── v1.0-baseline.txt
│   ├── v1.1-role-added.txt
│   └── v2.0-xml-structure.txt  ← current
├── task/
│   ├── summarize-v1.txt
│   └── summarize-v2.txt
└── CHANGELOG.md
```

Commit prompts to git with semantic versioning. CHANGELOG entries:
```
v2.0 (2026-03-06): Added XML structure, role definition, constraint block
  - Motivation: 12% quality improvement in eval
  - Tested against: 50-item test suite
  - Breaking: requires Claude 3+ (XML parsing)
```

### Step 2: Define test suite
For each critical prompt, minimum 10 test cases covering:
- Happy path (ideal input)
- Edge cases (empty input, very long input)
- Adversarial (injection attempts, conflicting instructions)
- Regression cases (historical failure modes)

Format:
```yaml
test_cases:
  - id: TC-001
    input: "Summarize this: [100-word text]"
    expected_behavior: "3-5 bullet summary"
    assertion: length < 100 words AND contains key topics
    pass_criterion: >= 4/5 criteria met
```

### Step 3: Automated eval pipeline (Promptfoo)
```yaml
# promptfoo.yaml
prompts:
  - id: current
    file: prompts/system/v2.0-xml-structure.txt
  - id: candidate
    file: prompts/system/v2.1-candidate.txt

providers:
  - id: anthropic:claude-sonnet-5

tests:
  - file: test-suite.yaml

sharing: false
```

Run: `npx promptfoo eval --no-cache` → generates comparison report.
CI integration: add to GitHub Actions pre-merge check.

### Step 4: Deployment workflow
```
Feature branch → Promptfoo eval → Score ≥ baseline → PR review → Staging test → Production deploy
                                  Score < baseline → reject, iterate
```

Never deploy prompt changes directly to production without eval comparison.

### Step 5: Production monitoring
Track per prompt version:
- Task completion rate
- User satisfaction / downstream metric
- Hallucination rate (if verifiable)
- Latency + cost

Rollback criterion: if metric drops >10% vs baseline → revert to previous version immediately.

### Step 6: OPRO integration (optional — automated optimization)
For prompts that can be auto-optimized:
1. Define objective metric (task score, user rating)
2. Run OPRO: LLM generates candidate prompts → evaluate → iterate
3. Treat OPRO output as a candidate prompt — run through Step 3 eval before deploying
4. Document OPRO run in CHANGELOG

## §4 — Verification Keys

- `VK [C-073]: prompt versioned — current: [vX.Y]`
- `VK [C-073]: test suite defined — N cases: [count]`
- `VK [C-073]: eval pipeline: configured/pending/skip`
- `VK [C-073]: deployment gate: eval comparison required`

## §5 — Anti-Patterns

- **Cowboy prompt deployment**: pushing prompt changes directly to prod without testing
- **No version history**: can't rollback when something breaks
- **Vanity metrics**: tracking "prompt quality" subjectively instead of downstream task performance
- **Test suite rot**: test cases that no longer reflect real-world use patterns
- **OPRO without grounding**: auto-optimizing against a poorly-defined objective → prompts that score well on metric but fail in production

## §6 — Cortex Log

```bash
ssh pafi@100.81.233.9 "curl -s -X POST http://localhost:6400/api/store \
  -H 'Content-Type: application/json' \
  -d '{\"text\":\"PROMPTWARE: [system] | prompt version: [vX.Y] | test suite: [N] cases | eval: [score vs baseline]\",
       \"collection\":\"procedures\",
       \"metadata\":{\"type\":\"promptware-engineering\",\"procedure\":\"C-073\"}}'"
```
