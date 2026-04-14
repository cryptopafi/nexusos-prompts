---
type: procedure
created: 2026-03-17
status: active
slug: security-audit-openclaw
tags: [procedure, nexus]
---

# Security Audit OpenClaw — Procedură Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.1
**Regulă asociată**: SEC-H-002 (Port 18789 NEVER public), SEC-H-006 (Richard Autonomy — Genie as Guardian)
**Scope**: Auditul periodic de securitate al sistemului OpenClaw — verificarea suprafeței de atac, configurației porturilor și expunerii neautorizate.

---

## 1. Problema

OpenClaw expune porturi locale și rulează agenți cu acces la tools reale (browser, filesystem, shell). Fără un audit periodic de securitate:
- Port 18789 ar putea fi expus public nedetectat (AGENT-HEALTH-MONITORING verifică la 15 min, dar nu auditul configurației)
- Configurații de securitate degradate peste timp (trustedProxies, session-key override)
- Extensii sau tools noi adăugate fără vetting (SEC-VET-001)
- Acumulare de sesiuni deschise sau auth tokens stale

**Situații acoperite**:
- Audit lunar standard al configurației OpenClaw
- Audit imediat post-incident (după orice alertă CRITICAL din AGENT-HEALTH-MONITORING)
- Audit pre-activare (înainte de a reactiva OpenClaw după HIBERNATING)
- Audit post-update (după update major OpenClaw)

---

## 2. Procedura

### Pas 1: Rulare Audit Nativ OpenClaw
Execută comanda de audit built-in și salvează output:

```bash
AUDIT_DATE=$(date +%Y%m%d)
AUDIT_FILE="$HOME/.openclaw/audits/audit-${AUDIT_DATE}.txt"
mkdir -p ~/.openclaw/audits

openclaw security audit 2>/dev/null > "$AUDIT_FILE" || \
openclaw security audit --deep 2>/dev/null > "$AUDIT_FILE" || \
echo "openclaw audit unavailable — manual checks required" > "$AUDIT_FILE"

cat "$AUDIT_FILE"
```

Interpretare output:
- `0 critical · 0 warn` → sistem curat
- Orice **CRITICAL** → acțiune imediată (vezi Pas 4)
- **WARN** → loghează și monitorizează
- **INFO** → documentează configurația curentă

### Pas 2: Port Audit Manual
Verifică binding-ul tuturor porturilor OpenClaw:

```bash
# Porturile trebuie să fie NUMAI pe 127.0.0.1 sau ::1
lsof -i :18789 | grep LISTEN
lsof -i :18791 | grep LISTEN
lsof -i :18792 | grep LISTEN
lsof -i :19001 | grep LISTEN
lsof -i :19003 | grep LISTEN
lsof -i :19004 | grep LISTEN
```

Criteriu PASS: toate adresele sunt `127.0.0.1` sau `localhost` sau `::1`.
Dacă oricare e `0.0.0.0` sau `*` → INCIDENT CRITIC → execută PORT-18789-LOCALHOST-ENFORCEMENT.md imediat.

### Pas 3: Config Integrity Check
Verifică configurația gateway:

```bash
# Verifică openclaw.json e valid JSON5 și fără modificări neașteptate
python3 -c "
import subprocess, json
result = subprocess.run(['openclaw', 'config', 'get'], capture_output=True, text=True)
if result.returncode == 0:
    print('Config accesibil')
    print(result.stdout[:500])
else:
    print('Config inaccesibil — verificare manuală necesară')
" 2>/dev/null || cat ~/.openclaw/openclaw.json | python3 -m json.tool > /dev/null && echo "JSON valid"
```

Verifică manual aceste câmpuri și aplică verdictul:

| Câmp | Valoare acceptată | Verdict dacă altfel |
|------|-------------------|---------------------|
| `gateway.trustedProxies` | gol `[]` sau absent (dacă GW nu e expus prin proxy) | WARN — documentează cine e proxy |
| `gateway.http.sessionKeyOverride` | `enabled: false` sau absent | WARN dacă `true` — API holders tratați ca trusted |
| `tools.elevated` | `enabled: false` sau absent | CRITICAL dacă `true` — agenții au acces la tools privilegiate |

### Pas 4: SOUL.md Integrity Full Scan
Verifică hash-ul tuturor 6 agenți (nu doar Richard ca în monitoring):

```bash
HASHES_FILE="$HOME/.nexus/soul-hashes.json"
AGENTS=("rich-new" "finance" "marketing" "legal" "ops" "insights")

for agent in "${AGENTS[@]}"; do
  SOUL_PATH="$HOME/.openclaw/workspaces/$agent/SOUL.md"
  EXPECTED=$(python3 -c "import json; d=json.load(open('$HASHES_FILE')); print(d['agents']['${agent//-new/}']['sha256'])" 2>/dev/null || echo "")
  CURRENT=$(shasum -a 256 "$SOUL_PATH" 2>/dev/null | awk '{print $1}' || echo "MISSING")

  if [ "$CURRENT" = "MISSING" ]; then
    echo "❌ MISSING: $agent SOUL.md"
  elif [ -z "$EXPECTED" ]; then
    echo "⚠️ NO BASELINE: $agent (hash: ${CURRENT:0:16}...)"
  elif [ "$CURRENT" = "$EXPECTED" ]; then
    echo "✅ OK: $agent"
  else
    echo "🚨 MODIFIED: $agent | expected: ${EXPECTED:0:16}... | got: ${CURRENT:0:16}..."
  fi
done
```

### Pas 5: Auth Token Audit
Verifică că token-urile de autentificare sunt valide și nu sunt stale:

```bash
# Verifică că token-ul Telegram e accesibil
security find-generic-password -s "TELEGRAM_BOT_TOKEN" -w 2>/dev/null | wc -c
# Trebuie să returneze > 10 (token non-gol)

# Verifică auth token OpenClaw (dacă configurat)
# security find-generic-password -s "OPENCLAW_AUTH_TOKEN" -w 2>/dev/null
```

### Pas 6: Generare Raport și Logging
Salvează raportul cu verdict final:

Operatorul (Genie) setează variabilele pe baza rezultatelor din Pașii 1-5, apoi generează raportul:

```bash
AUDIT_DATE=$(date +%Y%m%d)

# Setează pe baza rezultatelor pașilor anteriori:
PORT_RESULT="OK"       # OK | CRITICAL: portX pe 0.0.0.0
SOUL_RESULT="OK"       # OK | INCIDENT: agentX modified
CONFIG_RESULT="OK"     # OK | WARN: câmp problematic | CRITICAL: tools.elevated=true
AUTH_RESULT="OK"       # OK | WARN: token lipsă/stale

# Calculează verdict automat
if echo "$PORT_RESULT $SOUL_RESULT $CONFIG_RESULT $AUTH_RESULT" | grep -q "CRITICAL"; then
  VERDICT="CRITICAL"
elif echo "$PORT_RESULT $SOUL_RESULT $CONFIG_RESULT $AUTH_RESULT" | grep -q "WARN\|INCIDENT"; then
  VERDICT="WARNINGS"
else
  VERDICT="OK"
fi

# Scrie raport
cat >> ~/.openclaw/audits/audit-${AUDIT_DATE}.txt <<REPORT
--- Audit $AUDIT_DATE | Verdict: $VERDICT ---
Port audit: $PORT_RESULT
SOUL.md: $SOUL_RESULT
Config: $CONFIG_RESULT
Auth tokens: $AUTH_RESULT
REPORT
```

Salvează în Cortex (vezi §3).

---

## 3. Cortex Logging

```json
{
  "text": "OpenClaw Security Audit [DATA]. Ports: [OK/CRITICAL: portX]. SOUL.md: [OK/INCIDENT: agentX]. Config: [OK/WARN: descriere]. Auth tokens: [OK]. Verdict: [OK/WARNINGS/CRITICAL].",
  "collection": "sessions",
  "metadata": {
    "source_agent": "genie",
    "type": "security_audit",
    "procedure": "SECURITY-AUDIT-OPENCLAW",
    "rule_id": "SEC-H-002",
    "tags": ["security", "audit", "openclaw", "ports", "soul-md", "genie"]
  }
}
```

---

## 4. Enforcement Loop (META-H-002)

### WHERE
- Manual: Genie la cererea Pafi sau post-incident
- Automat: NIGHTLY-AUDIT-OPENCLAW Pas 1 acoperă subset (SOUL.md + port check)
- Recomandat: lunar sau după orice incident CRITICAL

### WHEN
- **Lunar**: primul marți al lunii (sau echivalent sesiune Genie)
- **Post-incident**: după orice alertă CRITICAL din AGENT-HEALTH-MONITORING
- **Pre-activare**: înainte de a porni OpenClaw după perioadă de HIBERNATING
- **Post-update**: după update major al OpenClaw sau al configurației gateway

### HOW (violation detection)
- Niciun raport în `~/.openclaw/audits/` din ultimele 30 zile → audit ratat → violation SEC-H-002
- Port 18789 expus public nedetectat → violation SEC-H-002 (CRITICAL)
- SOUL.md modificat și nedetectat > 24h → violation SEC-H-006
- Runner: manual Genie + NIGHTLY-AUDIT-OPENCLAW pentru subset zilnic

### CONNECT
- `SEC-H-002` → port 18789 NEVER public — verificat activ la Pas 2
- `SEC-H-006` → Genie guardian — SOUL.md full scan la Pas 4
- `PORT-18789-LOCALHOST-ENFORCEMENT.md` → apelat dacă Pas 2 detectează port expus
- `SOUL-MD-IMMUTABILITY.md` → apelat dacă Pas 4 detectează hash modificat
- `AGENT-HEALTH-MONITORING.md` → complementar (monitorizare continuă vs audit periodic)
- `NIGHTLY-AUDIT-OPENCLAW.md` → Pas 1 execută subset de checks din această procedură
- `procedure-health.json` → entry `"SECURITY-AUDIT-OPENCLAW": "active"`

### VERIFY
La verificarea configurației:
- [ ] `~/.openclaw/audits/` are rapoarte din ultimele 30 zile?
- [ ] Cel mai recent audit a inclus port check (Pas 2)?
- [ ] Cel mai recent audit a inclus SOUL.md scan (Pas 4)?
- [ ] Raportul a fost salvat în Cortex?
- [ ] VK emis în sesiune?
- [ ] Dacă oricare = NU → audit nu s-a desfășurat complet

**VK-uri obligatorii**:
1. `✅ [PROC] SECURITY-AUDIT-OPENCLAW | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Security Audit OpenClaw" | FORGE ✓ | rule: SEC-H-002 | v1.1`

### MODEL ROUTING
| Activitate | Model | Motivul |
|-----------|-------|---------|
| Pas 1-5 (checks) | Genie (Sonnet) | Orchestrare checks + interpretare output |
| Investigare finding critic | Opus subagent | Root cause analysis complex |
| Raport final | Genie (Sonnet) | Sinteză și recomandări |

---

## 5. Dependențe

| Componentă | Rol | Path/Endpoint |
|-----------|-----|---------------|
| `openclaw` CLI | Audit nativ | în PATH pe MacIntel |
| `~/.openclaw/audits/` | Arhivă rapoarte | `audit-YYYYMMDD.txt` per rulare |
| `soul-hashes.json` | Baseline SOUL.md | `~/.nexus/soul-hashes.json` |
| `PORT-18789-LOCALHOST-ENFORCEMENT.md` | Recovery port expus | `~/.nexus/procedures/openclaw/` |
| `SOUL-MD-IMMUTABILITY.md` | Recovery SOUL.md | `~/.nexus/procedures/openclaw/` |
| macOS Keychain | Auth tokens | `TELEGRAM_BOT_TOKEN` |
| `NIGHTLY-AUDIT-OPENCLAW.md` | Subset zilnic de checks | `~/.nexus/procedures/openclaw/` |
| `AGENT-HEALTH-MONITORING.md` | Monitorizare continuă complementară | `~/.nexus/procedures/openclaw/` |
| `procedure-health.json` | Registry status proceduri | `~/.nexus/procedure-health.json` |

---

## 6. Metrics

| Metrică | Ce măsoară | Target | Baseline (2026-02-26) |
|---------|-----------|--------|-----------------------|
| Audit frequency | Zile între audituri | < 30 zile | 6 zile (de la creare) |
| Critical findings | Issues CRITICAL per audit | 0 (trend descrescător) | 0 critical |
| Port exposure detection | Timp de la expunere la detectare | < 15 min (via AGENT-HEALTH-MONITORING) | N/A (netest.) |
| Audit coverage | % checks completate per audit | 100% (toți 6 pași) | 100% (audit inițial) |

---

## 7. Rapoarte Existente

| Data | Path | Verdict |
|------|------|---------|
| 2026-02-26 | `~/.openclaw/audits/audit-20260226.txt` | 0 critical, 1 warn, 2 info |

---

## Changelog

| Versiune | Data | Modificări |
|---------|------|-----------|
| 1.0 | 2026-03-04 | Creat — formalizează procesul de audit inițiat de Codex batch 040 (2026-02-26) ca procedură recurentă lunară |
| 1.1 | 2026-03-04 | FORGE-AUDIT: +3 dependențe lipsă (NIGHTLY-AUDIT, AGENT-HEALTH, procedure-health), +baseline metrici, Pas 3 tabel verdict explicit, Pas 6 script automat verdict |

## Checklist Pre-Publicare

- [x] Regulă asociată există (SEC-H-002, SEC-H-006)
- [x] Enforcement loop complet: WHERE + WHEN + HOW + CONNECT + VERIFY
- [x] VERIFY checkpoint prezent
- [x] Descrie CE nu CUM
- [x] VK format specificat
- [x] Entry adăugat în `procedure-health.json`
- [x] Dependencies tabel complet (toate referințele din §4 acoperite)
- [x] Metrics cu baseline (§6)
- [x] Prim audit executat (audit-20260226.txt, 0 critical)
- [x] Salvat în Cortex cu metadata FORGE
