---
type: procedure
created: 2026-03-20
status: active
slug: changelog
tags: [procedure, nexus]
---

# Procedures Framework — CHANGELOG

**Generated**: 2026-02-28
**Total file-based procedures**: 17

---

## v2.1 (1 procedure)

| Procedure | Rule | Created | Last Modified |
|-----------|------|---------|---------------|
| TRAINING-MODE.md | COM-H-001 | 2026-02-28 | 2026-02-28 |

## v1.4 (1 procedure)

| Procedure | Rule | Created | Last Modified |
|-----------|------|---------|---------------|
| FORGE-AUDIT.md | QUAL-H-004 | 2026-02-28 | 2026-02-28 |

## v1.3 (2 procedures)

| Procedure | Rule | Created | Last Modified |
|-----------|------|---------|---------------|
| FORGEBUILD.md | META-H-002 | 2026-02-27 | 2026-02-28 |
| ENRICH-PROCEDURE.md | ECHELON-S-001 | 2026-02-27 | 2026-02-27 |

## v1.0 (13 procedures)

| Procedure | Rule | Created | Last Modified |
|-----------|------|---------|---------------|
| AUDIT-IMPROVEMENT-CYCLE.md | META-H-003 | 2026-02-27 | 2026-02-27 |
| CODEX-BRIEF-PREWRITE-CHECKLIST.md | CODEX-H-001 | 2026-02-27 | 2026-02-27 |
| CODEX-ERROR-DETECTION.md | OPS-H-002 | 2026-02-27 | 2026-02-27 |
| CODEX-MODEL-ROUTING-PROCEDURE.md | COST-H-001 | 2026-02-27 | 2026-02-27 |
| CODEX-QUEUE-MONITORING.md | OPS-S-003 | 2026-02-27 | 2026-02-27 |
| CONTEXT-BLUEPRINT-AUDIT.md | META-H-001 | 2026-02-28 | 2026-02-28 |
| CONVERSATION-QUALITY-IMPROVEMENT.md | QUAL-H-004 | 2026-02-27 | 2026-02-27 |
| ERROR-RESPONSE-PROTOCOL.md | ERR-H-001 | 2026-02-27 | 2026-02-27 |
| PRECISION-MODE.md | COM-H-001 | 2026-02-28 | 2026-02-28 |
| PROCEDURE-TESTING-SYSTEM.md | META-H-001 | 2026-02-27 | 2026-02-27 |
| PROMPTFORGE-DAILY-AUDIT.md | PROMPT-H-002 | 2026-02-27 | 2026-02-27 |
| SOURCE-SELECTION-FRAMEWORK.md | REASON-H-001 | 2026-02-28 | 2026-02-28 |
| SUPERDELPHI-REFINE-BRIDGE.md | RESEARCH-S-001 | 2026-02-27 | 2026-02-27 |

---

## Tech Debt

### VK Emission Inconsistency

**Status**: OPEN
**Severity**: Medium
**Identified**: 2026-02-28

Verification Key (VK) emission is inconsistent across the procedures framework. Approximately 60% of procedures emit dual VKs (input VK + output VK) as intended by the FORGE template standard. The remaining ~40% either emit a single VK, emit none, or have inconsistent VK placement.

**Impact**: Without consistent VK emission, the enforcement loop cannot reliably verify that a procedure was both triggered correctly (input VK) and completed successfully (output VK). This weakens traceability and audit coverage.

**Recommended action**: Standardization pass across all 17 file-based procedures to enforce dual VK emission per FORGE template section 4.1. Target: 100% dual VK coverage.
