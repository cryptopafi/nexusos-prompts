---
type: procedure
created: 2026-03-17
status: active
slug: api-keys-rotation-openclaw
tags: [procedure, nexus]
---

# API Keys Rotation OpenClaw — Procedură Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-05
**Versiune**: 1.0
**Regulă asociată**: SEC-H-001 (Security Baseline), OPS-H-001 (Technical Autonomy)
**Scope**: Rotația periodică și post-incident a cheilor API și tokenilor folosiți de OpenClaw — ANTHROPIC_API_KEY, OPENAI_API_KEY, OPENROUTER_API_KEY, tokeni Telegram, Discord, Gateway auth. Înlocuiește `rotate-keys-reminder.sh` (reminder one-shot) cu o procedură completă.

---

## 1. Problema

OpenClaw folosește 15+ chei API și tokeni stocați în Keychain și `~/.openclaw/openclaw.json`. Fără o procedură de rotație:
- **Chei compromise nedetectate** — incident 2026-02-21: credențiale găsite în artifact-uri istorice, au necesitat rotație de urgență a 20+ chei
- **Rotație incompletă** — `rotate-keys-reminder.sh` era un reminder one-shot pentru o singură dată, nu o procedură repetabilă
- **Fără audit trail** — nu există log de când s-au rotit cheile și care
- **Expirare nedetectată** — API keys expirate cauzează eșecuri silențioase ale agenților

**Incidente documentate**:
- 2026-02-21: 20+ chei compromise (credențiale în plaintext în session artifacts). Document: `~/.openclaw/security/KEY_ROTATION_REQUIRED_2026-02-21.md`
- `rotate-keys-reminder.sh` → reminded doar pe 2026-02-22, NU e o procedură repetabilă

**Situații acoperite**:
- Rotație periodică planificată (lunar sau trimestrial)
- Rotație post-incident (credential leak, breach suspected)
- Rotație parțială (doar un subset de chei — ex: după schimbare provider)
- Verificare stare curentă a cheilor fără rotație

---

## 2. Procedura

### Pas 1: Inventar chei active
```bash
# Listează cheile stocate în Keychain pentru OpenClaw
security find-generic-password -a "ANTHROPIC_API_KEY" -s "openclaw-keys" -w 2>/dev/null && echo "anthropic: OK" || echo "anthropic: MISSING"
security find-generic-password -a "OPENAI_API_KEY" -s "openclaw-keys" -w 2>/dev/null && echo "openai: OK" || echo "openai: MISSING"
security find-generic-password -a "OPENROUTER_API_KEY" -s "openclaw-keys" -w 2>/dev/null && echo "openrouter: OK" || echo "openrouter: MISSING"
security find-generic-password -a "telegram-bot-claudeautomationbot" -s "telegram-token" -w 2>/dev/null && echo "telegram-genie: OK" || echo "telegram-genie: MISSING"
security find-generic-password -a "telegram-bot-claudeautomationbot" -s "openclaw-keys" -w 2>/dev/null && echo "telegram-rich: OK" || echo "telegram-rich: MISSING"

# Verifică data ultimei rotații
grep "KEY ROTATION" ~/.nexus/soul-change-log.md 2>/dev/null | tail -3 || echo "Niciun log de rotație găsit"
```

### Pas 2: Rotație chei AI (Anthropic, OpenAI, OpenRouter)

**NOTA**: Generează cheile noi din dashboard-urile respective ÎNAINTE de a rula comenzile de mai jos.

```bash
# Template — completează NEW_KEY_* cu valorile reale
NEW_ANTHROPIC_KEY="sk-ant-..."
NEW_OPENAI_KEY="sk-..."
NEW_OPENROUTER_KEY="sk-or-..."

# Actualizează Keychain
security add-generic-password -a "ANTHROPIC_API_KEY" -s "openclaw-keys" -w "$NEW_ANTHROPIC_KEY" -U
security add-generic-password -a "OPENAI_API_KEY" -s "openclaw-keys" -w "$NEW_OPENAI_KEY" -U
security add-generic-password -a "OPENROUTER_API_KEY" -s "openclaw-keys" -w "$NEW_OPENROUTER_KEY" -U

echo "Chei AI actualizate în Keychain"

# Actualizează și în openclaw.json dacă cheile sunt hardcodate acolo
# Verifică mai întâi
grep -c "sk-ant\|sk-or-\|ANTHROPIC" ~/.openclaw/openclaw.json && echo "ATENȚIE: Chei hardcodate în openclaw.json — actualizează manual!" || echo "openclaw.json: fără chei hardcodate OK"
```

### Pas 3: Rotație tokeni Telegram
```bash
# Token bot Telegram (generat din BotFather)
NEW_TELEGRAM_TOKEN="..."

security add-generic-password -a "telegram-bot-claudeautomationbot" -s "telegram-token" -w "$NEW_TELEGRAM_TOKEN" -U
echo "Token Telegram actualizat"

# Verifică că scriptul rich-hourly-report.sh funcționează cu noul token
bash ~/.openclaw/scripts/rich-hourly-report.sh
tail -1 ~/.openclaw/logs/rich-hourly-report.log
# Așteptat: [SENT] gw=... linie fără erori
```

### Pas 4: Rotație Gateway auth token
```bash
# Gateway token (din openclaw.json → auth)
NEW_GATEWAY_TOKEN=$(openssl rand -hex 32)
echo "Nou token Gateway: $NEW_GATEWAY_TOKEN"

# Actualizează în openclaw.json (folosind OPENCLAW-CONFIG-PATCH)
python3 << PYEOF
import json, os
home = os.path.expanduser("~")
with open(f"{home}/.openclaw/openclaw.json") as f:
    d = json.load(f)
# Backup înainte de modificare
import shutil, time
ts = time.strftime("%Y%m%d-%H%M%S")
shutil.copy(f"{home}/.openclaw/openclaw.json", f"{home}/.openclaw/openclaw.json.bak.{ts}")
# Actualizează auth token
if 'auth' in d and 'token' in d['auth']:
    d['auth']['token'] = "$NEW_GATEWAY_TOKEN"
    with open(f"{home}/.openclaw/openclaw.json", 'w') as f:
        json.dump(d, f, indent=2)
    print(f"Gateway token actualizat. Backup: openclaw.json.bak.{ts}")
else:
    print("auth.token nu există în openclaw.json — verifică manual structura")
PYEOF

# Restart Gateway după schimbare auth token
openclaw stop 2>/dev/null || launchctl bootout gui/$(id -u) ~/Library/LaunchAgents/ai.openclaw.gateway.plist 2>/dev/null || true
sleep 3
openclaw start 2>/dev/null || launchctl bootstrap gui/$(id -u) ~/Library/LaunchAgents/ai.openclaw.gateway.plist
sleep 5
openclaw health 2>/dev/null || curl -sf http://127.0.0.1:18789/health && echo "Gateway OK cu nou token"
```

### Pas 5: Revocare chei vechi
```bash
# IMPORTANT: Revocă cheile vechi din dashboard-urile respective:
# - Anthropic: https://console.anthropic.com/settings/keys
# - OpenAI: https://platform.openai.com/api-keys
# - OpenRouter: https://openrouter.ai/settings/keys
# Nu automatizabil — necesită acces manual la dashboard

echo "Checklist revocare:"
echo "[ ] Anthropic key veche revocată"
echo "[ ] OpenAI key veche revocată"
echo "[ ] OpenRouter key veche revocată"
echo "[ ] Token Telegram vechi dezactivat (dacă aplicabil)"
```

### Pas 6: Verificare post-rotație
```bash
# Test Anthropic API
curl -s -X POST https://api.anthropic.com/v1/messages \
  -H "x-api-key: $(security find-generic-password -a 'ANTHROPIC_API_KEY' -s 'openclaw-keys' -w 2>/dev/null)" \
  -H "anthropic-version: 2023-06-01" \
  -H "content-type: application/json" \
  -d '{"model":"claude-haiku-4-5-20251001","max_tokens":10,"messages":[{"role":"user","content":"ping"}]}' \
  | python3 -c "import json,sys; d=json.load(sys.stdin); print('Anthropic OK:', d.get('type',''))" 2>/dev/null || echo "Anthropic: EROARE"

# Test Gateway
openclaw health 2>/dev/null || curl -sf http://127.0.0.1:18789/health && echo "Gateway: OK" || echo "Gateway: DOWN"

# Verifică că niciun agent nu a eșuat
tail -5 ~/.openclaw/logs/genie-monitor.log | grep -i "error\|fail" || echo "Monitor log: fără erori recente"
```

### Pas 7: Logging rotație
```bash
ROTATION_DATE=$(date +%Y-%m-%d)
ROTATION_TS=$(date '+%Y-%m-%d %H:%M')

cat >> ~/.nexus/soul-change-log.md << EOF

## ${ROTATION_DATE} — API Keys Rotation — Rotație periodică/post-incident
- **Executat de**: Genie
- **Autorizat de**: Pafi
- **Chei rotite**: [lista cheilor rotite — ex: ANTHROPIC_API_KEY, TELEGRAM_TOKEN]
- **Motiv**: [periodic / post-incident / provider change]
- **Chei vechi revocate**: [DA/NU — și unde]
- **Verificare post-rotație**: [OK/PARTIAL]
- **Executat la**: ${ROTATION_TS}
EOF

echo "Log rotație adăugat în soul-change-log.md"
```

---

## 3. Cortex Logging

```json
{
  "text": "API Keys Rotation OpenClaw: rotatie executata la [DATA]. Chei rotite: [lista]. Motiv: [periodic/post-incident]. Chei vechi revocate: [DA/NU]. Verificare post-rotatie: [OK/PARTIAL]. Procedura API-KEYS-ROTATION-OPENCLAW aplicata.",
  "collection": "sessions",
  "metadata": {
    "source_agent": "genie",
    "type": "security_rotation",
    "procedure": "API-KEYS-ROTATION-OPENCLAW",
    "rule_id": "SEC-H-001",
    "tags": ["api-keys", "rotation", "security", "openclaw", "keychain", "governance"]
  }
}
```
**Nota**: Substituie `[DATA]`, `[lista]`, `[motiv]` cu valorile reale înainte de POST.

---

## 4. Enforcement Loop (META-H-002)

### WHERE
- Executat de Genie la cererea Pafi sau post-incident automat detectat
- Rotație periodică: marcată în calendar / trigger din NIGHTLY-AUDIT Pas 5 (security check)
- Nu e automat — necesită accesul Pafi la dashboard-urile API pentru revocare

### WHEN
- **Periodic**: trimestrial (Q1/Q2/Q3/Q4) sau lunar pentru chei cu risc mare
- **Post-incident**: imediat după orice suspected credential leak sau breach
- **Post-provider change**: la schimbarea unui provider AI (ex: migrare de la OpenRouter)
- **Trigger NIGHTLY-AUDIT**: dacă Pas 5 detectează chei expirate sau anomalii auth

### HOW (violation detection + acțiune post-violation)
- Chei vechi > 90 zile fără rotație → violation SEC-H-001
  - **Acțiune**: Genie inițiază această procedură și notifică Pafi pe Telegram cu lista cheilor scadente
- Agent eșuează cu `401 Unauthorized` sau `403 Forbidden` → cheie expirată/compromisă → violation
  - **Acțiune**: Genie identifică cheia din log-ul de eroare, inițiază rotație parțială Pas 2/3/4 pentru cheia respectivă
- `soul-change-log.md` fără entry `KEY ROTATION` > 90 zile → rotație omisă → violation
  - **Acțiune**: Genie notifică Pafi și programează sesiune de rotație în <48h

### CONNECT
- `SEC-H-001` → security baseline: cheile se rotesc periodic
- `OPS-H-001` → autonomie: Genie poate roti cheile AI fără intervenție Pafi (dar notifică)
- `OPENCLAW-CONFIG-PATCH.md` → Pas 4 folosește acest flux pentru patch la openclaw.json
- `SECURITY-AUDIT-OPENCLAW.md` → Pas 3 al auditului verifică starea cheilor
- `NIGHTLY-AUDIT-OPENCLAW.md` → Pas 5 poate detecta chei expirate
- `RICHARD-HOURLY-REPORT.md` → Pas 3 verifică că token Telegram funcționează post-rotație
- `procedure-health.json` → entry `"API-KEYS-ROTATION-OPENCLAW": "active"`

### VERIFY
- [ ] Toate cheile din Keychain returnează valori non-empty (`security find-generic-password ... -w`)?
- [ ] Test Anthropic API → 200 OK (Pas 6)?
- [ ] Gateway UP post-rotație token Gateway (`openclaw health`)?
- [ ] Chei vechi revocate din dashboard-urile respective (Pas 5 completat)?
- [ ] Entry în `soul-change-log.md` cu data și cheile rotite (Pas 7)?
- [ ] Cortex logging completat (substituție placeholders reale)?
- [ ] VK emis în sesiune?

**VK-uri obligatorii**:
1. `✅ [PROC] API-KEYS-ROTATION-OPENCLAW | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "API Keys Rotation OpenClaw" | FORGE ✓ | rule: SEC-H-001 | v1.0`

### MODEL ROUTING
| Activitate | Model | Motivul |
|-----------|-------|---------|
| Rotație chei (Pas 1-7) | Genie (Sonnet) | Comenzi Keychain + bash |
| Revocare dashboard | Pafi manual | Nu automatizabil |
| Diagnosticare 401/403 | Genie (Sonnet) | Log analysis + identificare cheie |

---

## 5. Dependențe

| Componentă | Rol | Path absolut |
|-----------|-----|--------------|
| macOS Keychain | Stocare chei API | Tool: `/usr/bin/security` |
| `~/.openclaw/openclaw.json` | Config cu Gateway auth token | `/Users/pafi/.openclaw/openclaw.json` |
| `~/.nexus/soul-change-log.md` | Audit trail rotații | `/Users/pafi/.nexus/soul-change-log.md` |
| `~/.openclaw/scripts/rich-hourly-report.sh` | Test token Telegram post-rotație | `/Users/pafi/.openclaw/scripts/rich-hourly-report.sh` |
| `~/.openclaw/security/KEY_ROTATION_REQUIRED_2026-02-21.md` | Referință incident 2026-02-21 | `/Users/pafi/.openclaw/security/KEY_ROTATION_REQUIRED_2026-02-21.md` |
| `OPENCLAW-CONFIG-PATCH.md` | Patch openclaw.json pentru Gateway token | `/Users/pafi/.nexus/procedures/openclaw/OPENCLAW-CONFIG-PATCH.md` |
| `SECURITY-AUDIT-OPENCLAW.md` | Audit periodic — detectare chei expirate | `/Users/pafi/.nexus/procedures/openclaw/SECURITY-AUDIT-OPENCLAW.md` |
| `procedure-health.json` | Registry proceduri active | `/Users/pafi/.nexus/procedures/openclaw/procedure-health.json` |

---

## 6. Metrics

| Metrică | Ce măsoară | Target | Metoda de măsurare | Frecvență |
|---------|-----------|--------|--------------------|-----------|
| Key rotation age | Zile de la ultima rotație | < 90 zile | `grep "KEY ROTATION" ~/.nexus/soul-change-log.md \| tail -1` → data | Lunar |
| Post-rotation API success rate | % API calls OK după rotație | 100% | Test Pas 6 → Anthropic 200 OK + Gateway UP | La fiecare rotație |
| Revocation completeness | % chei vechi revocate | 100% | Checklist Pas 5 completat | La fiecare rotație |
| Incident-triggered rotations | Rotații neplanificate (post-breach) | 0/an | Count entries `soul-change-log.md` cu motiv `post-incident` | Anual |

---

## Changelog

| Versiune | Data | Modificări |
|---------|------|------------|
| 1.0 | 2026-03-05 | Creat — înlocuiește `rotate-keys-reminder.sh` (one-shot reminder) cu procedură completă. Incident 2026-02-21 ca referință. |

## Checklist Pre-Publicare

- [x] Regulă asociată există (SEC-H-001, OPS-H-001)
- [x] Enforcement loop complet: WHERE + WHEN + HOW + CONNECT + VERIFY
- [x] Acțiune post-violation definită în HOW (3 scenarii: vârstă >90 zile, 401/403, log absent)
- [x] VERIFY checkpoint cu 7 itemi și comenzi concrete
- [x] VK format specificat
- [x] Entry adăugat în `procedure-health.json`
- [x] Pas 5 (revocare) marcat explicit ca manual (nu automatizabil)
- [x] Logging în soul-change-log.md (Pas 7) cu toate câmpurile necesare
- [x] Path-uri absolute în tabelul Dependențe
- [x] Metrics cu metodă de măsurare și frecvență
- [x] Incident 2026-02-21 documentat în §1
- [x] Nota substituție placeholders în Cortex JSON
