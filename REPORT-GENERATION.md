---
type: procedure
created: 2026-03-17
status: active
slug: report-generation
tags: [procedure, nexus]
---

# Report Generation — Master Procedure

**Status**: ACTIVE
**Created**: 2026-03-11
**Updated**: 2026-03-11
**Version**: 2.0
**Scope**: Standardized HTML report generation for all NexusOS agents and inter-agent communication.
**Template Library**: `~/.nexus/lib/report_template.py` (v1.0.0) — SINGLE SOURCE OF TRUTH for CSS, JS, and HTML components.

---

## 1. Report Generation Prompt (PromptForge Optimized)

**Score**: 82/100 | **Techniques**: C-048 Template, C-050 Recipe, C-042 Persona, C-071 Context Engineering

```
<report_generation>

<role>
You are generating a professional HTML report for Pafi's NexusOS infrastructure.
Your reports are read by a non-technical decision-maker who needs: clarity, action items, and evidence.
</role>

<design_system>
HARD RULES (violating any = report rejected):
1. Single-file self-contained HTML. No external CSS/JS/fonts. Everything embedded.
2. Semantic HTML: use <header>, <main>, <section>, <details>, <table>, <progress>, <meter>.
3. System fonts only: system-ui for headings, Charter/Cambria for body, ui-monospace for code.
4. Max-width 900px centered. White space: 2-3rem between sections.
5. Print CSS: @media print with page-break-inside: avoid on cards and tables.
6. Responsive: CSS Grid auto-fit, clamp() for typography, overflow-x: auto on tables.
7. Color = meaning: green (#16a34a) = ok, amber (#d97706) = caution, red (#dc2626) = risk, blue (#2563eb) = info.
8. Progressive disclosure: executive summary FIRST, details in collapsible <details> elements.
9. Every number needs: context (what), trend (vs what), status (RAG badge).
10. NO emojis. NO purple gradients. NO excessive rounded corners. NO Inter font. Vary alignment.

CSS VARIABLES (copy exactly):
:root {
  --c-bg: #ffffff; --c-surface: #f8f9fa; --c-border: #e2e8f0;
  --c-text: #1a202c; --c-text-muted: #64748b; --c-primary: #2563eb;
  --c-success: #16a34a; --c-warning: #d97706; --c-danger: #dc2626;
  --font-sans: system-ui, -apple-system, 'Segoe UI', Roboto, sans-serif;
  --font-serif: Charter, 'Bitstream Charter', 'Sitka Text', Cambria, serif;
  --font-mono: ui-monospace, 'Cascadia Code', 'Source Code Pro', Menlo, monospace;
  --space-xs: 0.25rem; --space-sm: 0.5rem; --space-md: 1rem;
  --space-lg: 1.5rem; --space-xl: 2rem; --space-2xl: 3rem;
  --content-width: 900px; --radius: 8px;
}
</design_system>

<components>
Use ONLY these component patterns:

KPI CARD (for top-level metrics):
<div class="kpi-grid">
  <div class="kpi-card">
    <div class="kpi-label">METRIC NAME</div>
    <div class="kpi-value">VALUE</div>
    <div class="kpi-delta positive|negative">CONTEXT</div>
  </div>
</div>

RAG BADGE (for status indicators):
<span class="rag-badge rag-green|rag-amber|rag-red|rag-blue">STATUS</span>

DATA TABLE (for structured data):
<table class="report-table">
  <thead><tr><th>COL</th></tr></thead>
  <tbody><tr><td>DATA</td></tr></tbody>
</table>

CALLOUT (for highlighted information):
<div class="callout callout-info|callout-warning|callout-success">CONTENT</div>

COLLAPSIBLE (for progressive disclosure):
<details class="collapsible">
  <summary>TITLE</summary>
  <div class="content">EXPANDED CONTENT</div>
</details>

TIMELINE (for sequential events):
<div class="timeline">
  <div class="timeline-item done|pending">
    <div class="timeline-time">HH:MM</div>
    <div class="timeline-title">EVENT</div>
    <div class="timeline-desc">DETAILS</div>
  </div>
</div>

PROGRESS BAR (for completion tracking):
<div style="background:var(--c-surface);border-radius:4px;overflow:hidden;height:8px">
  <div style="background:var(--c-success);width:75%;height:100%"></div>
</div>

INLINE SVG SPARKLINE (for micro-trends):
<svg width="80" height="20" viewBox="0 0 80 20">
  <polyline points="0,18 16,12 32,15 48,8 64,10 80,3" fill="none" stroke="var(--c-primary)" stroke-width="2"/>
</svg>
</components>

<structure>
Every report MUST follow this skeleton:

1. TITLE + subtitle (date, generator, context)
2. EXECUTIVE SUMMARY (callout-info, 2-3 sentences max)
3. KPI CARDS (3-5 top metrics, grid layout)
4. MAIN CONTENT (sections with h2 headings, tables, timelines as needed)
5. FINDINGS / RECOMMENDATIONS (if audit/analysis type)
6. APPENDIX (collapsible details for raw data, sources, methodology)
7. FOOTER (generator, file path, timestamp)

Report types and their specific sections:
- BUILD REPORT: timeline + files changed + test results + remaining work
- AUDIT REPORT: NPLF scores + findings table + recommendations + files audited
- RESEARCH REPORT: sources table + key findings + synthesis + knowledge gaps
- PIPELINE REPORT: stages + pass/fail per stage + timing + bottlenecks
- INTELLIGENCE REPORT: signals table + priority ranking + action items + sources
- BENCHMARK REPORT: comparison table + delta columns + winner per metric + methodology
- INTER-AGENT REPORT: agent status grid + handoffs + pending items + health
</structure>

<quality_gates>
Before delivering a report, verify:
- [ ] Executive summary fits in 3 sentences
- [ ] Every number has context + trend + RAG status
- [ ] All tables have hover states and zebra striping via CSS
- [ ] Collapsible sections work (details/summary)
- [ ] Print preview renders correctly (no cut tables/cards)
- [ ] No external dependencies (all CSS/JS inline)
- [ ] File size under 100KB (no Base64 images unless essential)
- [ ] Contrast ratio: text on backgrounds passes WCAG AA
- [ ] No AI slop: no centered everything, no gradient backgrounds, no uniform roundness
</quality_gates>

</report_generation>
```

---

## 2. Agent Reporting Matrix

| Agent | Report Types | Frequency | Output Path |
|-------|-------------|-----------|-------------|
| **Lis** (Claude Code) | Build, Audit, Research, Pipeline, Benchmark | Per task | `~/.nexus/workspace/output/` |
| **Codex** | Build (delivery summary) | Per task | `~/.codex/codex-to-genie.md` + HTML optional |
| **TECH** | Audit | Per audit | `~/.nexus/workspace/output/forge/` |
| **IRIS** | Research, Intelligence | Per research cycle | `~/.nexus/workspace/output/iris/` |
| **Delphi** | Research (academic) | Per query | `~/.nexus/workspace/output/delphi/` |
| **ECHELON** | Intelligence (daily feed) | Daily | `~/.nexus/workspace/output/echelon/` |
| **RADAR** | Intelligence (business) | Weekly/on-demand | `~/.nexus/workspace/output/radar/` |
| **SENTINEL** | Pipeline (health), Inter-Agent | Every 10 min | `~/.nexus/workspace/intel/` |
| **Inter-Agent** | Handoff, Status, Benchmark | On handoff / daily | `~/.nexus/workspace/output/interagent/` |

---

## 3. Inter-Agent Report Protocol

When one agent completes work that another agent needs:

```
HANDOFF REPORT (required for cross-agent task transfer):
- Source agent + task ID
- Deliverables (file paths, status)
- Outstanding issues (blockers, warnings)
- Quality score (if audited)
- Next steps for receiving agent

FORMAT: HTML if >5 data points, Markdown if <5
PATH: ~/.nexus/workspace/output/interagent/{date}-{source}-to-{target}.html
```

---

## 4. Benchmark Report Template

For the 2-week agentic vs subagent comparison:

```
BENCHMARK ENTRY (per task):
- Task ID + description
- Method A (agentic): time, token cost, quality score, tool calls
- Method B (subagent): time, token cost, quality score, tool calls
- Delta: time saved, cost delta, quality delta
- Winner + reasoning
- Confidence: HIGH/MEDIUM/LOW

Aggregate weekly into benchmark summary report.
PATH: ~/.nexus/workspace/output/benchmarks/
```

---

## 5. Unified Template Library — `~/.nexus/lib/report_template.py`

**REPLACES** the old CSS-only approach. All HTML generators MUST import from this library.

### Architecture
```
~/.nexus/lib/report_template.py       ← Single source of truth (CSS + JS + Python API)
    ├── CSS_CORE                       ← GitHub Dark design system, WCAG AA, fluid typography
    ├── _js_share(url)                 ← Internal: share button JS with embedded VPS URL
    ├── wrap_html(title, body, meta)   ← Main entry point — produces complete HTML (includes JS)
    ├── badge(text, variant)           ← Status badges (pass/warn/fail/info/neutral)
    ├── epr_badge(score)               ← EPR circular badge with color tiers
    ├── metric_card(label, value, ...) ← KPI metric cards
    ├── data_table(headers, rows, ...) ← Styled tables with zebra/hover/tabular-nums
    ├── section(title, content, ...)   ← Section wrapper (collapsible option)
    ├── callout(text, variant)         ← Info/warning/success/error callouts
    ├── upload_to_vps(path, name)      ← SCP to VPS, returns public URL
    └── publish_html(title, body, ...) ← All-in-one: generate + embed URL + write + upload
```

### Generators migrated to template
| Generator | File | Status |
|-----------|------|--------|
| Nexus Research | `~/.nexus/echelon/nexus-html-render.py` | MIGRATED v2.0 |
| Radar Light Digest | `~/.nexus/scripts/radar-light-digest.py` | MIGRATED v2.0 |
| Morning Brief | `~/.nexus/scripts/morning-brief.py` | MIGRATED v2.0 |
| Skill Optimizer | `~/.claude/plugins/skill-creator/.../generate_report.py` | PENDING |
| Ad-hoc reports | (Claude Code sessions) | USE `wrap_html()` |

### Usage for new reports
```python
import sys
from pathlib import Path
sys.path.insert(0, str(Path(__file__).resolve().parent.parent / "lib"))
from report_template import wrap_html, section, metric_card, data_table, badge, callout

body = section("Summary", callout("Report generated successfully.", "success"))
body += section("Metrics", '<div class="metric-grid">' + metric_card("Score", "95%", "+5%", "pass") + '</div>')
html = wrap_html("My Report", body, meta={"generator": "MyTool", "date": "2026-03-11"})
```

### Share Button
Every report automatically includes a Share button (top-right, fixed position) that:
1. Uses Web Share API (mobile/desktop where supported)
2. Falls back to Clipboard API
3. Falls back to `execCommand('copy')`
4. Shows toast "Link copied!" for 2 seconds
5. Hidden on print

### Self-Optimization
- LaunchAgent: `com.nexus.report-optimizer` (daily 4:00 AM)
- Script: `~/.nexus/scripts/report-template-optimizer.sh`
- Status: `~/.nexus/logs/report-optimizer-status.json`
- Checks: syntax validation, migration progress, report count, Telegram alerts on issues

---

## 6. Enforcement Loop

- **Rule**: REP-H-001 — All HTML reports MUST use `report_template.py`
- **Check**: `report-template-optimizer.sh` daily scan (4 AM)
- **Alert**: Telegram if generators found with inline CSS not importing template
- **Metric**: migration percentage (target: 100%)

---

## References
- Template library: `~/.nexus/lib/report_template.py`
- Demo report: `~/.nexus/workspace/output/template-demo.html`
- Self-optimizer: `~/.nexus/scripts/report-template-optimizer.sh`
- PromptForge: `~/.nexus/procedures/PROMPTING.md`
