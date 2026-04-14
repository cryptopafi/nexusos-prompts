---
type: procedure
created: 2026-03-17
status: active
slug: agent-browser-nexus
tags: [procedure, nexus]
---

# Agent Browser Nexus Integration

**Status**: DRAFT
**Motiv status**: Procedura documentează pașii de instalare dar agent-browser NU a fost instalat pe MacM4 (Pas 1-6 neexecutați). Se marchează ACTIVE după execuția VERIFY §3 cu succes.
**Versiune**: 1.1
**Creat**: 2026-03-04
**Domeniu**: Infrastructure / AI Tooling
**Regulă asociată**: META-H-002
**Scope**: Instalare și integrare Vercel Agent Browser în Nexus pentru Genie (Bash), Codex (self-verification) și ECHELON (no-API scraping). Include Electron app control (Notion, VS Code).

---

## §1 CONTEXT

**Ce este**: CLI browser agentic open-source (Apache 2.0) de la Vercel Labs. Wraps Playwright cu un sistem "snapshot + refs" care returnează doar referințe compacte ale elementelor interactive (`@e1`, `@e2`) în loc de DOM complet.

**De ce contează pentru Nexus**:
- **93% mai puțini tokeni** vs Playwright MCP (raportat de Pulumi/Vercel, neverificat independent — benchmark intern)
- **Electron control**: controlează Notion, VS Code, Discord via Chrome DevTools Protocol (CDP)
- **No-API workflows**: ECHELON poate scrapa site-uri fără RSS/API
- **Codex self-verification**: Codex verifică UI-ul propriilor deployments
- **Genie direct**: rulează via Bash tool fără OpenClaw

**Stats**: GitHub 18.5K stars, v0.16.3, lansat 2026-01-11, 260 open issues.

---

## §2 PREREQUISITE CHECK

- [ ] Node.js >= 18 instalat pe MacM4
- [ ] npm disponibil (`npm --version`)
- [ ] ~300MB spațiu disk liber (Chromium download)
- [ ] Notion/VS Code instalate dacă e nevoie de Electron control

---

## Pas 1 — Instalare

```bash
# Install global
npm install -g agent-browser

# Download Chromium engine (~300MB, o singură dată)
agent-browser install

# Verificare
agent-browser --version
agent-browser open https://example.com
agent-browser snapshot -i
agent-browser close
```

**CHECKPOINT 1**: `--version` returnează v0.16.x+. Snapshot returnează `button "..." [ref=e1]` format.

---

## Pas 2 — Claude Code Skill

```bash
# Navighează la proiect sau home
cd ~/

# Instalează skill core
npx skills add vercel-labs/agent-browser

# Instalează skill Electron (Notion, VS Code, Discord)
npx skills add vercel-labs/agent-browser --skill electron

# Verificare
ls -la .claude/skills/agent-browser/SKILL.md
```

**CHECKPOINT 2**: Genie poate executa `agent-browser open <url>` și `agent-browser snapshot -i` via Bash tool.

**✅ Sintaxă verificată**: `npx skills add vercel-labs/agent-browser --skill electron` este sintaxa oficială confirmată de Vercel ([sursa](https://x.com/ctatedev/status/2028128730132922760)). Flag-ul `--skill` selectează sub-skill-ul din repo.

**⚠️ SECURITY GATE — ClawHub**: Agent Browser vine de la vercel-labs GitHub oficial, NU ClawHub. Skill-urile instalate prin `npx skills add vercel-labs/...` sunt sigure. NU instala skills necunoscute de pe ClawHub fără audit manual.

---

## Pas 3 — Security Config

```bash
# Adaugă în ~/.zshrc (idempotent — nu adaugă duplicate)
if ! grep -q "AGENT_BROWSER_ALLOWED_DOMAINS" ~/.zshrc; then
cat >> ~/.zshrc << 'EOF'

# Agent Browser — Nexus Security Config
export AGENT_BROWSER_ALLOWED_DOMAINS="notion.so,*.notion.so,reddit.com,*.reddit.com,youtube.com,*.youtube.com,localhost,127.0.0.1,100.81.233.9"
export AGENT_BROWSER_CONTENT_BOUNDARIES=1
export AGENT_BROWSER_MAX_OUTPUT=50000
EOF
fi

source ~/.zshrc
```

**✅ Env vars verificate**: `AGENT_BROWSER_ALLOWED_DOMAINS`, `AGENT_BROWSER_CONTENT_BOUNDARIES`, `AGENT_BROWSER_MAX_OUTPUT` sunt documentate oficial pe [agent-browser.dev/configuration](https://agent-browser.dev/configuration).

**CHECKPOINT 3**: `env | grep AGENT_BROWSER` returnează toate cele 3 variabile. Navigare la domeniu neautorizat returnează eroare.

---

## Pas 4 — Electron Control (Notion + VS Code)

### Creare launch scripts

```bash
mkdir -p ~/.nexus/scripts

# Notion cu CDP
cat > ~/.nexus/scripts/launch-notion-cdp.sh << 'EOF'
#!/bin/bash
osascript -e 'quit app "Notion"' 2>/dev/null; sleep 2
open -a "Notion" --args --remote-debugging-port=9226
echo "Notion launched with CDP on port 9226"
EOF

# VS Code cu CDP
cat > ~/.nexus/scripts/launch-vscode-cdp.sh << 'EOF'
#!/bin/bash
osascript -e 'quit app "Visual Studio Code"' 2>/dev/null; sleep 2
open -a "Visual Studio Code" --args --remote-debugging-port=9223
echo "VS Code launched with CDP on port 9223"
EOF

chmod +x ~/.nexus/scripts/launch-notion-cdp.sh
chmod +x ~/.nexus/scripts/launch-vscode-cdp.sh
```

### Test Electron connection

```bash
bash ~/.nexus/scripts/launch-notion-cdp.sh
sleep 4
agent-browser connect 9226 --session notion
agent-browser snapshot -i --session notion
agent-browser close --session notion
```

**CHECKPOINT 4**: Snapshot-ul Notion returnează element refs (titluri, butoane, inputs). Dacă returnează eroare de conexiune, încearcă `--cdp 9226` în loc de `connect`.

**⚠️ SECURITY NOTE — CDP Credential Exposure**: Snapshot-urile CDP pot expune token-uri de autentificare din Notion/Slack (cookies, session tokens vizibile în DOM). NU loga sau salva snapshot-uri raw în locuri accesibile public. Folosește `--session` izolat per task și `agent-browser close` imediat după terminare.

**Port map**:
| App | Port |
|-----|------|
| Notion | 9226 |
| VS Code | 9223 |
| Discord | 9224 |
| Figma | 9225 |
| Slack | 9222 |

---

## Pas 5 — ECHELON No-API Scraping

```bash
# Wrapper script pentru ECHELON
cat > ~/.nexus/scripts/echelon-browser-scrape.sh << 'EOF'
#!/bin/bash
# Usage: echelon-browser-scrape.sh <url> [session-name] [output-dir]
URL="$1"
SESSION="${2:-echelon-$(date +%s)}"
OUTDIR="${3:-/tmp/echelon}"

mkdir -p "$OUTDIR"
agent-browser open "$URL" --session "$SESSION"
agent-browser wait --load networkidle --session "$SESSION"
agent-browser snapshot -i --session "$SESSION" > "$OUTDIR/snapshot.txt"
agent-browser screenshot --full --session "$SESSION" "$OUTDIR/screenshot.png"

# Extract text content
agent-browser get text "body" --session "$SESSION" > "$OUTDIR/text.txt" 2>/dev/null || true

agent-browser close --session "$SESSION"
echo "Done: $OUTDIR/snapshot.txt + screenshot.png"
EOF

chmod +x ~/.nexus/scripts/echelon-browser-scrape.sh
```

**Test**:
```bash
bash ~/.nexus/scripts/echelon-browser-scrape.sh "https://reddit.com/r/cryptocurrency" echelon-test /tmp/echelon-test
cat /tmp/echelon-test/snapshot.txt | head -20
```

**CHECKPOINT 5**: Script returnează `snapshot.txt` cu element refs și `screenshot.png` negoal.

---

## Pas 6 — Codex Self-Verification

Codex adaugă după fiecare deployment:

```bash
# ~/.nexus/scripts/codex-verify.sh
#!/bin/bash
PORT="${1:-3000}"
OUTDIR="/tmp/codex-verify-$(date +%s)"
mkdir -p "$OUTDIR"

agent-browser open "http://localhost:$PORT"
agent-browser wait --load networkidle
agent-browser snapshot -i > "$OUTDIR/snapshot.txt"
agent-browser screenshot --full "$OUTDIR/screenshot.png"
agent-browser close

echo "Verification complete: $OUTDIR/"
ls -la "$OUTDIR/"
```

**Codex include în delivery**: `bash ~/.nexus/scripts/codex-verify.sh 3000`

**CHECKPOINT 6**: Codex delivery include dovadă de verificare (snapshot + screenshot path).

---

## §3 VERIFY — Checklist Final

```bash
# Rulează toate checkpoints
agent-browser --version                                    # v0.16.x+
ls .claude/skills/agent-browser/SKILL.md                  # skill instalat
env | grep AGENT_BROWSER_ALLOWED_DOMAINS                   # security config
env | grep AGENT_BROWSER_CONTENT_BOUNDARIES                # = 1
bash ~/.nexus/scripts/launch-notion-cdp.sh && sleep 4 && \
  agent-browser connect 9226 --session test && \
  agent-browser snapshot -i --session test && \
  agent-browser close --session test                      # Electron OK
bash ~/.nexus/scripts/echelon-browser-scrape.sh \
  "https://example.com" test-echelon /tmp/verify-test    # ECHELON OK
```

**Pass criteria**: Toate 6 comenzi returnează output valid fără erori.

---

## §3b CORTEX LOGGING

La fiecare sesiune agent-browser, salvează în Cortex:
```bash
curl -s -X POST http://100.81.233.9:6400/api/store \
  -H "Content-Type: application/json" \
  -d '{
    "text": "[AGENT-BROWSER] session={session} url={url} tokens={estimate} outcome={ok/error}",
    "collection": "procedures",
    "metadata": {
      "type": "session-log",
      "tool": "agent-browser",
      "forge_bypass": true,
      "forge_bypass_reason": "session-log"
    }
  }'
```

---

## §4 ENFORCEMENT

**Regulă**: La orice task care implică web scraping sau interacțiune cu UI desktop, Genie verifică dacă agent-browser e mai potrivit decât alternativa curentă.

**Decision gate**:
```
Task implică web interaction?
├─ Playwright MCP e instalat? → DA → compară token usage
│   ├─ Page simplă / agent loop lung → agent-browser (93% mai puțin)
│   └─ Nevoie de multi-browser / Firefox/WebKit → Playwright
├─ Electron app (Notion, VS Code)? → agent-browser --cdp
└─ Site cu anti-bot / CAPTCHA? → Browserbase (cloud, plătit)
```

**VK obligatoriu**: La orice sesiune agent-browser: `[AGENT-BROWSER] session=<name> url=<url> tokens=<estimate>`

### WHERE
- WISH Step H — orice task care implică web interaction sau UI desktop
- ECHELON pipeline — la fiecare scraping task
- Codex delivery — self-verification post-deploy

### WHEN
- Înainte de a folosi Playwright MCP (compară token usage)
- La orice Electron app interaction (Notion, VS Code, Slack)
- La orice site fără API public

### HOW (violation detection)
- Task de web scraping folosit fără a evalua agent-browser → violation
- Playwright folosit pe pagină simplă când agent-browser e instalat → violation
- Sesiune agent-browser fără VK `[AGENT-BROWSER]` → violation

### CONNECT
- `META-H-002 (FORGE)` — procedura respectă template-ul FORGE
- `ECHELON` — integrare directă cu pipeline-ul de scraping

---

## §5 KNOWN ISSUES

1. **`--state`/`--profile` crash pe macOS ARM64** — confirmat v0.8.3, status neclar v0.16.x. Nu folosi state persistence până la confirmare.
2. **Rate limiting manual** — nu există built-in. ECHELON scripts trebuie să adauge `sleep 2` între requests.
3. **CDP Notion BROKEN pe v0.16.3** — `agent-browser --cdp 9226 snapshot -i` eșuează pe Electron CDP (`Failed to connect via CDP`) chiar când `/json` și `/json/version` răspund OK. Root cause observat: handshake incompatibil cu Origin header pe WebSocket CDP în Notion/Electron. Confirmat Codex m4-253 (2026-03-04).
4. **Known Issue: agent-browser --cdp atârnă cu Notion Electron**
   **Status**: WORKAROUND ACTIV
   **Comandă funcțională**: `python3 ~/.nexus/scripts/notion-snapshot.py`
   **Motiv**: agent-browser CDP handshake incompatibil cu Electron 39 (Notion). Python websockets direct funcționează.
5. **~260 open GitHub issues** — tool în dezvoltare activă. Pin versiunea în producție: `npm install -g agent-browser@0.16.3`
6. **Chromium detection** — `navigator.webdriver=true` e detectabil de anti-bot systems. Nu folosi pentru scraping agresiv.

---

## §6 ROLLBACK

```bash
npm uninstall -g agent-browser
rm -rf .claude/skills/agent-browser
# Șterge din ~/.zshrc liniile AGENT_BROWSER_*
# Șterge ~/.nexus/scripts/launch-*-cdp.sh
# Șterge ~/.nexus/scripts/echelon-browser-scrape.sh
# Șterge ~/.nexus/scripts/codex-verify.sh
```

---

## §7 REFERINȚE

- GitHub: `github.com/vercel-labs/agent-browser` (Apache 2.0)
- Docs: `agent-browser.dev`
- Skills: `npx skills add vercel-labs/agent-browser --skill electron`
- Pulumi benchmark (93% claim, neverificat independent): `pulumi.com/blog/self-verifying-ai-agents-vercels-agent-browser-in-the-ralph-wiggum-loop`
- npm: `npm install -g agent-browser` (nu `@vercel/agent-browser`)
