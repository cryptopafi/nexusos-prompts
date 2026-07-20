---
type: procedure
created: 2026-03-22
status: active
slug: c-063-etac-pipeline
tags: [procedure, nexus]
---

# Apply the Extract-Transform-AI-Create (ETAC) Pipeline — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 2.0
**Regulă asociată**: META-H-002
**Scope**: End-to-end data pipeline using pandas, numpy, and LLMs to extract raw data from heterogeneous sources, transform it into AI-ready format, run analysis with appropriate models, and create validated deliverables.

---

## 1. Problema

Data analysis with AI fails when raw data is fed directly to models without proper extraction, cleaning, and structuring. The ETAC pipeline separates concerns into reproducible stages, each with specific Python tooling and validation gates, ensuring AI receives clean, contextually relevant data.

**When to use**:
- Analyzing financial reports, sales data, or operational metrics with AI assistance
- Building automated reporting pipelines that combine structured data with LLM narrative
- Processing batch data from multiple heterogeneous sources (APIs, CSVs, databases, PDFs)
- Creating dashboards or executive summaries from raw data

**When NOT to use**:
- Real-time streaming data (use Apache Kafka/Flink pipelines instead)
- Simple single-file analysis that fits in one pandas operation — just use pandas directly
- When data is already clean and structured — skip to the AI step
- Exploratory data analysis where you do not yet know what you are looking for — use notebooks first

---

## 2. Procedura

### Pas 1: Extract — Gather raw data with provenance tracking

```python
import pandas as pd
import json
from datetime import datetime

# Multi-source extraction with metadata
sources = []

# Source 1: SQL database
df_sales = pd.read_sql("SELECT * FROM sales WHERE date >= '2026-01-01'", conn)
sources.append({"name": "sales_db", "rows": len(df_sales), "extracted_at": datetime.now().isoformat()})

# Source 2: CSV export
df_costs = pd.read_csv("costs_q1_2026.csv", parse_dates=["date"])
sources.append({"name": "costs_csv", "rows": len(df_costs), "extracted_at": datetime.now().isoformat()})

# Source 3: API
import requests
resp = requests.get("https://api.example.com/metrics", headers={"Authorization": f"Bearer {token}"})
df_api = pd.DataFrame(resp.json()["data"])
sources.append({"name": "metrics_api", "rows": len(df_api), "extracted_at": datetime.now().isoformat()})

# Provenance log
with open("extraction_log.json", "w") as f:
    json.dump({"sources": sources, "total_rows": sum(s["rows"] for s in sources)}, f, indent=2)
```

### Pas 2: Transform — Clean, normalize, and chunk for AI context

```python
import numpy as np

def transform_for_ai(df, context_window_tokens=100000, reserve_pct=0.20):
    """Clean data and chunk for AI processing."""
    # Step 1: Clean
    df = df.drop_duplicates()
    df = df.dropna(subset=["revenue", "date"])  # critical columns only
    df["date"] = pd.to_datetime(df["date"])
    df["revenue"] = pd.to_numeric(df["revenue"], errors="coerce")

    # Step 2: Normalize
    from sklearn.preprocessing import StandardScaler
    numeric_cols = df.select_dtypes(include=[np.number]).columns
    scaler = StandardScaler()
    df[numeric_cols] = scaler.fit_transform(df[numeric_cols])

    # Step 3: Chunk for context window
    max_tokens = int(context_window_tokens * (1 - reserve_pct))
    chars_per_token = 4  # approximate
    max_chars = max_tokens * chars_per_token

    chunks = []
    csv_str = df.to_csv(index=False)
    for i in range(0, len(csv_str), max_chars):
        chunk = csv_str[i:i + max_chars]
        chunks.append({
            "chunk_id": len(chunks),
            "char_count": len(chunk),
            "data": chunk,
            "rows_approx": chunk.count("\n"),
        })

    return chunks

chunks = transform_for_ai(df_sales)
print(f"Created {len(chunks)} chunks from {len(df_sales)} rows")
```

### Pas 3: Validate Transform — Pre-AI quality gate

```python
def validate_transform(df_original, chunks):
    """Ensure no data loss during transformation."""
    total_rows = sum(c["rows_approx"] for c in chunks)
    original_rows = len(df_original)

    checks = {
        "no_data_loss": total_rows >= original_rows * 0.95,
        "no_empty_chunks": all(c["char_count"] > 0 for c in chunks),
        "chunks_within_limit": all(c["char_count"] < 400000 for c in chunks),
    }

    for check, passed in checks.items():
        status = "PASS" if passed else "FAIL"
        print(f"  {check}: {status}")

    return all(checks.values())

assert validate_transform(df_sales, chunks), "Transform validation failed"
```

### Pas 4: AI — Run analysis with model routing

```python
# Model selection based on task type
ANALYSIS_ROUTING = {
    "summarize": {"model": "claude-sonnet-5", "reason": "Fast narrative generation"},
    "classify": {"model": "claude-haiku-4-5", "reason": "High volume, simple task"},
    "anomaly_detect": {"model": "claude-opus-4-8", "reason": "Nuanced pattern recognition"},
    "predict_trend": {"model": "gpt-4o", "reason": "Strong at numerical reasoning"},
}

# Process each chunk with consistent prompt
results = []
for chunk in chunks:
    prompt = f"""Analyze this sales data chunk ({chunk['chunk_id']+1}/{len(chunks)}):

{chunk['data']}

Tasks:
1. Identify top 3 revenue drivers
2. Flag any anomalies (values >2 std dev from mean)
3. Summarize trends in 3 bullet points

Output as JSON with keys: drivers, anomalies, trends"""

    # response = client.messages.create(model=..., messages=[{"role": "user", "content": prompt}])
    # results.append(json.loads(response.content[0].text))
```

### Pas 5: Create — Assemble deliverable from AI outputs

```python
import matplotlib.pyplot as plt

def create_deliverable(results, output_type="executive_report"):
    """Assemble AI outputs into final deliverable."""
    # Merge results across chunks
    all_drivers = [d for r in results for d in r.get("drivers", [])]
    all_anomalies = [a for r in results for a in r.get("anomalies", [])]
    all_trends = [t for r in results for t in r.get("trends", [])]

    if output_type == "executive_report":
        report = f"""# Q1 2026 Sales Analysis

## Key Revenue Drivers
{chr(10).join(f'- {d}' for d in all_drivers[:5])}

## Anomalies Detected
{chr(10).join(f'- {a}' for a in all_anomalies)}

## Trends
{chr(10).join(f'- {t}' for t in all_trends)}

---
*Generated: {datetime.now().strftime('%Y-%m-%d %H:%M')} | Sources: {len(sources)} | Chunks processed: {len(results)}*
"""
        return report

    elif output_type == "dashboard_data":
        return {"drivers": all_drivers, "anomalies": all_anomalies, "trends": all_trends}
```

### Pas 6: Validate Output — Post-AI quality gate

```python
def validate_output(results, ground_truth_sample=None):
    """Verify AI output accuracy on a sample."""
    # Check structural completeness
    for i, r in enumerate(results):
        assert "drivers" in r, f"Chunk {i}: missing drivers"
        assert "anomalies" in r, f"Chunk {i}: missing anomalies"
        assert "trends" in r, f"Chunk {i}: missing trends"

    # If ground truth available, check accuracy
    if ground_truth_sample:
        correct = sum(1 for gt, pred in zip(ground_truth_sample, results[:len(ground_truth_sample)])
                      if gt["top_driver"] in pred["drivers"])
        accuracy = correct / len(ground_truth_sample)
        print(f"Sample accuracy: {accuracy:.1%}")
        assert accuracy >= 0.85, f"Accuracy {accuracy:.1%} below 85% threshold — re-check prompts"

    return True
```

### Pas 7: Log and Archive — Reproducibility record

```python
pipeline_log = {
    "timestamp": datetime.now().isoformat(),
    "sources": sources,
    "chunks_created": len(chunks),
    "model_used": "claude-sonnet-5",
    "analysis_type": "revenue_analysis",
    "validation_passed": True,
    "error_rate": "2.3%",
    "deliverable_type": "executive_report",
    "prompts_hash": "sha256:abc123...",  # hash of all prompts for reproducibility
}
```

---

## 3. Cortex Logging

```json
{
  "text": "ETAC Pipeline executed for [dataset]. Extract: [X sources, Y rows]. Transform: [Z chunks]. AI: [model] for [analysis type]. Create: [deliverable type]. Validation: [error rate %].",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "C-063",
    "domain": "ai-data-analysis",
    "rule_id": "META-H-002",
    "tags": ["etac-pipeline", "data-analysis", "pandas", "extract-transform", "ai-pipeline"],
    "has_enforcement_loop": true,
    "forge_version": "2.0"
  }
}
```

---

## 4. Enforcement Loop

### WHERE
WISH Step H + Training Mode execution

### WHEN
Any data analysis project using AI for insights extraction

### HOW (violation detection)
- Pipeline executed without all 7 steps → violation
- Output without VK → violation
- Validation step (Pas 6) skipped → critical violation
- Prompts not logged for reproducibility → violation
- Data fed to AI without transform/cleaning → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- Training Mode → curriculum ai-data-analysis

### VERIFY
- [ ] All 7 steps executed?
- [ ] Transform validation passed?
- [ ] Output validation passed?
- [ ] Prompts logged for reproducibility?
- [ ] VK emitted?

**VK-uri:**
1. `✅ [PROC] C-063 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Apply the ETAC Pipeline" | FORGE ✓ | rule: META-H-002 | v2.0`

### MODEL ROUTING
| Activitate | Model | Justification |
|-----------|-------|---------------|
| Data summarization | Sonnet | Fast, good narrative |
| Anomaly detection | Opus | Nuanced pattern recognition |
| Classification | Haiku | High volume, simple decisions |
| Trend prediction | GPT-4o / Sonnet | Numerical reasoning |
| Validation audit | Opus | Critical quality gate |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| pandas, numpy | Data extraction and transformation |
| scikit-learn | StandardScaler, preprocessing |
| matplotlib/plotly | Visualization in deliverables |
| LLM API (Claude/GPT) | AI analysis step |
| Ground truth sample | Output validation |
| Cortex | Logging and archival |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% steps |
| VK emission | 2/2 |
| Transform validation | PASS |
| Output error rate | < 5% |
| Prompts logged | 100% |
| Reproducibility | Pipeline re-runnable from log |

---

## Changelog

| Versiune | Data | Modificări |
|---------|------|-----------|
| 1.0 | 2026-03-04 | Initial version |
| **2.0** | **2026-03-20** | Opus v2 rewrite: 7 steps with full Python code (pandas, numpy, sklearn), model routing table, pre/post validation gates, provenance tracking, chunking strategy, when to use/NOT use |
