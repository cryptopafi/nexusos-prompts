---
id: C-075
title: Agentic Tool-Use Prompting
domain: ai-prompt-engineering
version: 1.0
forge_audit: PASS
created: 2026-03-06
source: dontriskit/awesome-ai-system-prompts (14 production systems: Manus, v0, Cline, Bolt.new, Claude Code, same.new) | ReAct DAIR (arxiv) | ART DAIR | PE INSIGHT EPR 16/20
---

# C-075 Agentic Tool-Use Prompting — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-06
**Versiune**: 1.0
**Regula asociata**: PROMPT-H-002
**Scope**: Structurarea prompts-urilor pentru agenti cu tool use — extras din analiza a 14 sisteme de productie. Acopera: pre-planning phase, ONE-tool-per-iteration, Three-Tier approval, tool schema inline, environment grounding.

---

## §1 — Problema

Fara acest pattern, prompturile pentru agenti cu tool use sufera de:
- **Cascading failures**: agentul executa multiple tool calls in serie fara a verifica intermediar, amplificand erorile initiale
- **Planning hallucination**: agentul actioneaza direct fara a considera toate fisierele/sistemele afectate
- **Tool schema mismatch**: agentul incearca tool calls cu parametri gresiti pentru ca nu are sintaxa exacta in context
- **Environment incompatibility**: agentul genereaza comenzi incompatibile cu OS/runtime-ul real (ex: PowerShell syntax pe Linux)
- **Approval ambiguity**: agentul nu stie ce actiuni necesita confirmare umana vs ce poate face autonom

Aceasta procedura acopera:
- Prompturi pentru agenti cu tool use (bash, file system, API calls, browser automation)
- Agent loops multi-step (ReAct pattern in productie)
- System prompts de productie cu tool calling schemas
- Agenti autonomi cu potential impact destructiv (stergeri, modificari irreversibile)

**Diferenta critica fata de teoria ReAct**: Academia descrie ReAct ca interleaving fluid (Thought→Action→Observation). Productia (Manus, Cline, Bolt.new, Claude Code) adauga gate-uri rigide de confirmare si ONE-tool-per-iteration pentru a preveni cascading failures — constrangeri pe care teoria nu le mandateaza.

---

## §2 — Procedura

### Pas 1: Environment Grounding (obligatoriu, primul bloc din system prompt)
Include intotdeauna la inceputul system prompt-ului:
```
Environment:
- OS: {linux/macos/windows} {version}
- Working directory: {absolute/path}
- Date: {YYYY-MM-DD}
- Runtime: {node/python/bash} {version}
- Network: {available/restricted}
- Permissions: {what the agent can/cannot access}
```

Rationale: fara aceasta, agentul poate genera comenzi incompatibile cu mediul real (pattern din Manus, Cline, Bolt.new, same.new — prezent in 100% din sistemele de productie analizate).

### Pas 2: Pre-Planning Phase (obligatorie inainte de prima actiune)
Adauga inainte de loop-ul de actiuni:
```
Before taking any action, think HOLISTICALLY:
- What is the complete goal?
- What files, systems, or states will be affected?
- What are the edge cases and failure modes?
- What is the minimum set of actions needed?
- What is reversible vs irreversible?

Only after completing this analysis, begin tool calls.
```

Sursa: Bolt.new pattern ("Think HOLISTICALLY before acting. Consider all files that may need to be modified.").

### Pas 3: ONE Tool Per Iteration (anti-cascading-failure gate)
Adauga in sectiunea RULES sau BEHAVIOR a system prompt-ului:
```
CRITICAL: Make ONLY ONE tool call per response. After each tool call, wait for
the result before deciding on the next action. Do NOT chain multiple tool calls
in a single response.

Integrate the tool result into your understanding before the next step.
```

Aceasta constrangere apare explicit in Manus AgentLoop, Cline, same.new si Claude Code system prompts. Este cea mai consistenta divergenta fata de teoria ReAct academica.

### Pas 4: Three-Tier Approval Flow
Defineste explicit in system prompt ce necesita aprobare si ce nu:

```
ACTION AUTHORIZATION:
Tier 1 — Do without asking (safe, reversible):
  - Read files, list directories, search, view logs
  - Create new files in designated workspace
  - Run read-only queries

Tier 2 — Get explicit approval before doing (moderate impact):
  - Modify existing files
  - Install packages or dependencies
  - Make API calls with side effects
  - Run scripts that generate output affecting other systems

Tier 3 — NEVER do under any circumstances:
  - Delete files outside designated workspace
  - Modify system configuration files
  - Actions that cannot be undone without backup
  - [list specific forbidden actions for your context]
```

Sursa: Clawdbot pattern (cel mai formalizat din corpus). Adapteaza listele la contextul specific al agentului.

### Pas 5: Tool Schema Inline
Nu descrie doar ce face tool-ul — include sintaxa exacta in system prompt:

Exemplu XML-style (Cline pattern):
```xml
<tool_calls>
  <read_file>
    <path>/absolute/path/to/file.txt</path>
  </read_file>

  <write_file>
    <path>/absolute/path/to/output.txt</path>
    <content>file content here</content>
  </write_file>

  <bash>
    <command>ls -la /path</command>
  </bash>
</tool_calls>
```

Sau JSON-style (ChatGPT/v0 pattern):
```json
{
  "tool": "bash",
  "parameters": {
    "command": "string — shell command to execute"
  }
}
```

Principiu: modelul trebuie sa stie exact sintaxa call-ului, nu doar descrierea functionalitatii.

### Pas 6: Observation Integration
Dupa fiecare tool response, modelul TREBUIE sa integreze rezultatul explicit:
```
After receiving a tool result:
1. State what the result tells you
2. Update your mental model of the current state
3. Decide: does the result change your plan?
4. Only then: determine the next tool call (or report completion)
```

### Test Cases

1. **Normal flow** — Agent de file refactoring:
   - Aplica: Environment grounding (OS+cwd+date) + pre-planning ("ce fisiere sunt afectate?") + ONE-tool-per-iteration + Three-Tier (read=Tier1, modify=Tier2) + tool schema inline (read_file, write_file)
   - Output asteptat: agent cere aprobare inainte de a modifica fisiere, face UN tool call pe raspuns, integreaza rezultatul inainte de urmatorul pas

2. **Edge case** — Agent cu tool failure la primul call:
   - Tool call returneaza eroare (fisier inexistent)
   - Comportament asteptat: agentul integreaza eroarea ("file not found at path X"), ajusteaza planul ("I'll search for the correct path first"), face UN nou tool call (search, nu direct write)

3. **Failure case** — Prompt simplu one-shot fara tool calls:
   - Nu aplica acest pattern — overhead nejustificat
   - Comportament asteptat: ignora pre-planning phase si Three-Tier; foloseste promptul simplu

---

## §3 — Cortex Logging

```json
{
  "text": "C-075 Agentic Tool-Use Prompting aplicat: agent=[descriere] | tools=[lista tools] | three-tier=[YES/NO] | schema-inline=[YES/NO] | environment-grounded=[YES/NO]",
  "collection": "technical",
  "metadata": {
    "type": "procedure_artifact",
    "procedure": "C-075",
    "rule_id": "PROMPT-H-002",
    "has_enforcement_loop": true,
    "forge_version": "1.4",
    "tags": ["agentic", "tool-use", "react", "one-tool-per-iteration", "three-tier-approval", "production-agent", "ai-prompt-engineering"]
  }
}
```

---

## §4 — Enforcement Loop (META-H-002)

### WHERE
- PromptForge v2.3 — Task Type "Agentic/Tool-Use" (10th type, recomandat de PE INSIGHT Rec. 1)
- WISH Step H cand output-ul e un agent system prompt cu tool calling
- Orice sesiune in care Genie construieste un prompt pentru un agent autonom

### WHEN
- La FIECARE constructie de system prompt pentru agent cu tool use
- La detectia de cuvinte cheie in request: "agent", "tool use", "automation", "multi-step", "loop", "agentic"
- La review periodic al agentilor existenti (verificare ca respecta Three-Tier)

### HOW (violation detection)
- System prompt de agent fara Environment Grounding → violation
- System prompt de agent fara ONE-tool-per-iteration constraint → violation (risc cascading failure)
- System prompt de agent fara Three-Tier Approval → violation (risc actiuni destructive neautorizate)
- Tool calls descrise prin functionalitate fara sintaxa exacta → violation
- Runner: checklist manual la constructia system prompt-ului; audit Opus periodic

### CONNECT
- PROMPT-H-002 (hard rule) → agentic prompting e subset de prompt engineering obligatoriu
- C-059-react-prompting-pattern.md → teoria ReAct; aceasta procedura adauga gate-urile de productie
- C-071-context-engineering.md → context engineering acopera window design; aceasta acopera tool loop
- PROMPTING.md → master entry point pentru orice task de prompting
- `procedure-health.json` → adauga entry C-075

### VERIFY
- [ ] Environment Grounding prezent (OS, cwd, date, runtime)?
- [ ] Pre-Planning Phase definita in prompt?
- [ ] ONE-tool-per-iteration constraint explicit?
- [ ] Three-Tier Approval definit cu liste concrete?
- [ ] Tool schemas inline cu sintaxa exacta?
- [ ] Daca oricare = NU → procedura NU e completa

**VK-uri obligatorii** (per VK-H-001):
1. `[C-075] Agentic Tool-Use Prompting | env-grounded=YES | one-tool=YES | three-tier=YES | schema-inline=YES | DONE`
2. `[CORTEX] "C-075 Agentic Tool-Use Prompting aplicat" | collection: technical | type: procedure_artifact | SAVED`

### MODEL ROUTING

| Activitate | Model | Motivul |
|-----------|-------|---------|
| Constructie system prompt agentic | Sonnet 4.6 (Genie orchestrator) | Patternuri clare, template structural |
| Audit agent system prompt existent | Opus 4.6 subagent | Detectare gap-uri subtile in Three-Tier, cascading risks |
| Review Three-Tier pentru agenti noi | Sonnet 4.6 | Definire liste Tier 1/2/3 cu context specific |

---

## §4b — Note pentru Proceduri care Genereaza Prompturi

Aceasta procedura produce system prompts pentru agenti de productie:
- Referentiaza C-073 (Promptware Engineering) pentru versioning si eval gate obligatoriu
- Test suite minim pentru agenti: 5 cazuri (happy path, tool failure, authorization escalation, injection attempt, reversibility test)
- Rollback criterion: daca agentul trece de Tier 2 fara aprobare → revert system prompt imediat
- Referentiaza C-071 (Context Engineering) pentru design-ul complet al context window-ului agentului

---

## §5 — Dependente

| Componenta | Rol | Path/Endpoint |
|-----------|-----|---------------|
| C-059 ReAct Prompting Pattern | Teoria de baza (Thought→Action→Observation) | `~/.nexus/procedures/training/ai-prompt-engineering/C-059-react-prompting-pattern.md` |
| C-071 Context Engineering | Design context window agent | `~/.nexus/procedures/training/ai-prompt-engineering/C-071-context-engineering.md` |
| C-073 Promptware Engineering | Versioning si eval gate pentru system prompts | `~/.nexus/procedures/training/ai-prompt-engineering/C-073-promptware-engineering.md` |
| PROMPTING.md | Master entry point | `~/.nexus/procedures/PROMPTING.md` |
| dontriskit/awesome-ai-system-prompts | Corpus de 14 production system prompts (sursa primara) | GitHub (extern) |

---

## §6 — Metrics

| Metrica | Ce masoara | Target |
|---------|-----------|--------|
| Cascading failure rate | % tool call sequences cu eroare propagata nedetectata | 0% |
| Tier 2 compliance | % actiuni Tier 2 care au primit aprobare umana | 100% |
| Environment mismatch rate | % tool calls incompatibile cu OS/runtime actual | 0% |
| Schema error rate | % tool calls cu parametri gresiti din cauza sintaxei incorecte | 0% |

---

## Checklist Pre-Publicare

- [x] Regula asociata: PROMPT-H-002 (exista in rules-hard.md)
- [x] Enforcement loop complet: WHERE + WHEN + HOW + CONNECT + VERIFY
- [x] VERIFY checkpoint cu checks obligatorii
- [x] Doua VK-uri specificate (per VK-H-001)
- [x] Test cases documentate (3: normal, edge, failure)
- [x] Descrie CE nu CUM (zero cod inline >10 linii)
- [x] Cortex metadata: rule_id, type: procedure_artifact, has_enforcement_loop: true, forge_version: "1.4"
- [x] §4b note pentru prompturi in productie

---

## Changelog

| Versiune | Data | Modificari |
|---------|------|-----------|
| 1.0 | 2026-03-06 | Versiune initiala. Sursa: PE INSIGHT EPR 16/20, analiza 14 production system prompts (Manus, v0, Cline, Bolt.new, Claude Code, same.new), ReAct DAIR, ART DAIR. |
