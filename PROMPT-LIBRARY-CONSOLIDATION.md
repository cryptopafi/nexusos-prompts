---
type: procedure
created: 2026-03-19
status: active
slug: prompt-library-consolidation
tags: [procedure, nexus]
---

# Prompt Library Consolidation — Procedură FORGE

**Status**: EXECUTED + AUDITED
**Creat**: 2026-03-19
**Versiune**: 1.1
**Owner**: Genie
**Scope**: Centralizarea tuturor prompturilor LLM într-un singur folder (`~/.nexus/prompt-library/`) cu symlinks inverse în locațiile originale. Zero downtime, rollback garantat.

---

## 1. Problema

Prompturile sistemului NexusOS sunt răspândite în **7 locații distincte** pe disk. Consecințe:
- **Discovery incompletă**: SOL scanează doar 4 din 7 locații
- **Duplicate divergente**: `lis-system-prompt-v2.md` (memory) ≠ `system.xml` (relay) — drift nedetectat
- **Audit fragmentat**: FORGE-AUDIT trebuie să știe 7 path-uri diferite
- **Backup parțial**: `tar` pe o locație ratează restul

---

## 2. Inventar complet (auditat 2026-03-19)

| # | Locație curentă | Fișiere | Status | Risc mutare | Consumatori |
|---|-----------------|---------|--------|-------------|-------------|
| L1 | `memory/agent-prompts/` | 12 .md | LIVE | MEDIUM | SOL discover.py (glob), Genie dispatch |
| L2 | `~/.nexus/agents/*/SOUL.md` | 5 main | LIVE | HIGH | SOL discover.py (glob), smoke-test.sh (shasum), rebuild-registry.sh, soul-hashes.json |
| L3 | `~/.nexus/agents/tech/sub-agents/*/SOUL.md` | 4 sub | LIVE | HIGH | **NU sunt descoperite de SOL** (glob `*/SOUL.md` = 1 nivel) |
| L4 | `~/.nexus/procedures/training/ai-prompt-engineering/` | 116 .md | REFERENCE | LOW | PROMPTING.md (doc), notion-push-training.py (relative paths), 15 training skills |
| L5 | `memory/promptforge*.md` | 3 | REFERENCE | LOW | CLAUDE.md (doc ref), /opt skill |
| L6 | `memory/lis-system-prompt-v2.md` | 1 | REFERENCE | LOW | Memory doc only — relay loads `system.xml` separat |
| L7 | `memory/openclaw-configs/souls/` | 8 backup | REFERENCE | LOW | Deployment scripts (cp to VPS) |
| L8 | `relay/src/prompts/system.xml` (Lis) | 1 | LIVE | HIGH | relay.ts readFile (relative to PROJECT_ROOT) |
| L9 | `kara-telegram-relay/src/prompts/system.xml` | 1 | LIVE | HIGH | Kara relay |
| L10 | `~/.nexus/scripts/*.py` (embedded) | 22 | LIVE | **SKIP** | Tightly coupled — necesită refactoring separat |

**Cortex** (620+ prompturi, tag `prompting-library`): deja centralizat, nu se mută.

---

## 3. Structura țintă

```
~/.nexus/prompt-library/
├── INDEX.md                    ← Inventar complet, generat automat
├── patterns/                   ← Deja există (PATTERN-INDEX.md, P1-P69)
├── agents/                     ← 12 agent classification prompts (din L1)
├── souls/                      ← 9 SOUL.md: genie, tech, iris, mercury, sentinel,
│                                  builder, fixer, integrator, pipeliner (din L2+L3)
├── openclaw/                   ← 8 OpenClaw soul backups (din L7)
├── bots/
│   ├── lis-system-prompt.md    ← Memory doc (din L6)
│   ├── lis-system.xml          ← CANONICAL copy (din L8)
│   ├── kara-system.xml         ← CANONICAL copy (din L9)
│   └── luna-system.xml         ← Când va exista
├── techniques/                 ← 116 PE procedures (din L4)
└── methodology/                ← promptforge*.md, training plan (din L5)
```

**Principiu**: Fișierele ORIGINALE stau în `prompt-library/`. Locațiile vechi devin **symlinks**.

---

## 4. Dependențe critice (din audit)

| Consumator | Path pattern | Symlink-safe? | Acțiune necesară |
|------------|-------------|---------------|------------------|
| `prompt-optimizer-discover.py` | `Path.glob("*.md")` / `Path.glob("*/SOUL.md")` | ✅ DA | Re-run discover.py post-consolidation |
| `manifest.json` + `queue.json` | Absolute paths cached | ❌ NU — stale | Re-run discover.py → regenerează |
| `soul-hashes.json` | Absolute paths + shasum | ❌ NU — stale | Re-run `rebuild-registry.sh` |
| `nexus-smoke-test.sh` | `$HOME/.nexus/agents/$agent/SOUL.md` | ✅ DA | Symlink resolve transparent |
| `rebuild-registry.sh` | `$agent_dir/SOUL.md` | ✅ DA | Symlink resolve transparent |
| `notion-push-training.py` | Relative paths `"training/ai-prompt-engineering/"` | ⚠️ DEPINDE | Test post-symlink obligatoriu |
| `relay.ts` (Lis) | `readFile(join(PROJECT_ROOT, "src", "prompts", "system.xml"))` | ✅ DA | Symlink works |
| OpenClaw gateway configs | Hardcoded VPS paths `/home/openclaw/...` | ❌ N/A | **NU mutăm pe VPS** — doar backup local |
| CLAUDE.md, PROMPTING.md, skills | Documentation refs | ✅ DA | Update paths în docs la final |

---

## 5. Procedura (7 etape)

**Regulă transversală**: După fiecare `ln -s`, înregistrează symlink-ul creat. Helper function:
```bash
MANIFEST="$HOME/.nexus/prompt-library/.symlink-manifest"
track_symlink() { echo "$1" >> "$MANIFEST"; }
```
Fiecare `ln -s ... TARGET` este urmat de `track_symlink "TARGET"`.

### Etapa 0 — BACKUP COMPLET (risc: 0)

```bash
BACKUP_DIR="$HOME/.nexus/backups"
BACKUP_FILE="$BACKUP_DIR/prompt-library-pre-consolidation-$(date +%Y%m%d-%H%M%S).tar.gz"
mkdir -p "$BACKUP_DIR"

tar czf "$BACKUP_FILE" \
  "$HOME/.claude/projects/-Users-pafi/memory/agent-prompts/" \
  "$HOME/.nexus/agents/genie/SOUL.md" \
  "$HOME/.nexus/agents/tech/SOUL.md" \
  "$HOME/.nexus/agents/iris/SOUL.md" \
  "$HOME/.nexus/agents/mercury/SOUL.md" \
  "$HOME/.nexus/agents/sentinel/SOUL.md" \
  "$HOME/.nexus/agents/tech/sub-agents/" \
  "$HOME/.nexus/procedures/training/ai-prompt-engineering/" \
  "$HOME/.claude/projects/-Users-pafi/memory/promptforge.md" \
  "$HOME/.claude/projects/-Users-pafi/memory/promptforge-v3.md" \
  "$HOME/.claude/projects/-Users-pafi/memory/promptforge-training.md" \
  "$HOME/.claude/projects/-Users-pafi/memory/lis-system-prompt-v2.md" \
  "$HOME/.claude/projects/-Users-pafi/memory/openclaw-configs/souls/" \
  2>/dev/null

# Verificare
tar tzf "$BACKUP_FILE" | wc -l  # Așteptat: ~160+ fișiere
echo "Backup: $BACKUP_FILE ($(du -h "$BACKUP_FILE" | cut -f1))"
```

**Gate**: Backup valid (>150 fișiere) → continuă. Altfel → STOP.

### Etapa 1 — CREAZĂ STRUCTURA (risc: 0)

```bash
mkdir -p ~/.nexus/prompt-library/{agents,souls,openclaw,bots,techniques,methodology}

# Inițializează manifest symlinks (pentru rollback safe)
touch ~/.nexus/prompt-library/.symlink-manifest
```

Nu mută nimic, doar creează folderele.

### Etapa 2 — LOW RISK: Reference files (L4, L5, L6, L7)

**Ordine**: cele mai sigure primele. Niciuna nu e consumată live de un proces.

**2a. Training techniques (L4 → techniques/)**
```bash
# Mut fișierele
mv ~/.nexus/procedures/training/ai-prompt-engineering/*.md \
   ~/.nexus/prompt-library/techniques/

# Curăță hidden files (.DS_Store etc.) înainte de rmdir
find ~/.nexus/procedures/training/ai-prompt-engineering -maxdepth 1 -name '.*' -not -name '.' -delete 2>/dev/null

# Symlink directory
rmdir ~/.nexus/procedures/training/ai-prompt-engineering/  # doar dacă e gol
ln -s ~/.nexus/prompt-library/techniques \
   ~/.nexus/procedures/training/ai-prompt-engineering
track_symlink "$HOME/.nexus/procedures/training/ai-prompt-engineering"
```

**Test 2a**:
```bash
cat ~/.nexus/procedures/training/ai-prompt-engineering/C-042-persona-pattern.md | head -1
# Așteptat: prima linie a fișierului (nu eroare)
ls -la ~/.nexus/procedures/training/ai-prompt-engineering/
# Așteptat: symlink → ~/.nexus/prompt-library/techniques
```

**2b. PromptForge methodology (L5 → methodology/)**
```bash
for f in promptforge.md promptforge-v3.md promptforge-training.md; do
  mv "$HOME/.claude/projects/-Users-pafi/memory/$f" \
     "$HOME/.nexus/prompt-library/methodology/$f"
  ln -s "$HOME/.nexus/prompt-library/methodology/$f" \
     "$HOME/.claude/projects/-Users-pafi/memory/$f"
  track_symlink "$HOME/.claude/projects/-Users-pafi/memory/$f"
done
```

**Test 2b**:
```bash
cat ~/.claude/projects/-Users-pafi/memory/promptforge.md | head -3
# Așteptat: "## PromptForge v3.7"
```

**2c. Lis system prompt doc (L6 → bots/)**
```bash
mv "$HOME/.claude/projects/-Users-pafi/memory/lis-system-prompt-v2.md" \
   "$HOME/.nexus/prompt-library/bots/lis-system-prompt.md"
ln -s "$HOME/.nexus/prompt-library/bots/lis-system-prompt.md" \
   "$HOME/.claude/projects/-Users-pafi/memory/lis-system-prompt-v2.md"
track_symlink "$HOME/.claude/projects/-Users-pafi/memory/lis-system-prompt-v2.md"
```

**2d. OpenClaw soul backups (L7 → openclaw/)**
```bash
mv "$HOME/.claude/projects/-Users-pafi/memory/openclaw-configs/souls/"*.md \
   "$HOME/.nexus/prompt-library/openclaw/"
# Symlink înapoi
for f in "$HOME/.nexus/prompt-library/openclaw/"*.md; do
  fname=$(basename "$f")
  ln -sf "$f" "$HOME/.claude/projects/-Users-pafi/memory/openclaw-configs/souls/$fname"
  track_symlink "$HOME/.claude/projects/-Users-pafi/memory/openclaw-configs/souls/$fname"
done
```

**Gate Etapa 2**: Toate 4 teste PASS → continuă. Orice FAIL → rollback din backup.

### Etapa 3 — MEDIUM RISK: Agent prompts (L1 → agents/)

**Pre-audit**:
- SOL discover.py folosește `Path.glob("*.md")` pe acest director → symlink-safe
- manifest.json cache absolute paths → trebuie re-scan

```bash
# Mut fișierele
mv "$HOME/.claude/projects/-Users-pafi/memory/agent-prompts/"*.md \
   "$HOME/.nexus/prompt-library/agents/"

# Curăță hidden files (.DS_Store etc.) înainte de rmdir
find "$HOME/.claude/projects/-Users-pafi/memory/agent-prompts" -maxdepth 1 -name '.*' -not -name '.' -delete 2>/dev/null

# Symlink directory (nu individual files)
rmdir "$HOME/.claude/projects/-Users-pafi/memory/agent-prompts/"
ln -s "$HOME/.nexus/prompt-library/agents" \
   "$HOME/.claude/projects/-Users-pafi/memory/agent-prompts"
track_symlink "$HOME/.claude/projects/-Users-pafi/memory/agent-prompts"
```

**Test 3**:
```bash
# Symlink resolve
ls -la "$HOME/.claude/projects/-Users-pafi/memory/agent-prompts"
# Așteptat: symlink → ~/.nexus/prompt-library/agents

# Content accessible
cat "$HOME/.claude/projects/-Users-pafi/memory/agent-prompts/classifier.md" | head -1
# Așteptat: conținut valid

# SOL discover funcționează
python3 -c "
from pathlib import Path
p = Path.home() / '.claude/projects/-Users-pafi/memory/agent-prompts'
files = list(p.glob('*.md'))
print(f'Found {len(files)} agent prompts')
assert len(files) == 12, f'Expected 12, got {len(files)}'
print('PASS')
"

# Regenerează manifest (stale paths)
cd ~/.nexus && python3 scripts/prompt-optimizer-discover.py
```

**Gate Etapa 3**: 12 fișiere găsite via symlink + discover.py PASS → continuă.

### Etapa 4 — HIGH RISK: SOUL files (L2 + L3 → souls/)

**Pre-audit**: smoke-test.sh, rebuild-registry.sh, soul-hashes.json toate folosesc path-uri care trec prin symlink. Dar hashes devin stale.

```bash
# Atomic guard: dacă orice comandă fail → rollback imediat
set -e
trap 'echo "❌ Etapa 4 FAIL la $agent/$sub — rulează Etapa 7 rollback"; exit 1' ERR

# Mut main souls (5)
for agent in genie tech iris mercury sentinel; do
  mv "$HOME/.nexus/agents/$agent/SOUL.md" \
     "$HOME/.nexus/prompt-library/souls/$agent.md"
  ln -s "$HOME/.nexus/prompt-library/souls/$agent.md" \
     "$HOME/.nexus/agents/$agent/SOUL.md"
  track_symlink "$HOME/.nexus/agents/$agent/SOUL.md"
  echo "  ✓ $agent"
done

# Mut sub-agent souls (4)
for sub in builder fixer integrator pipeliner; do
  if [ -f "$HOME/.nexus/agents/tech/sub-agents/$sub/SOUL.md" ]; then
    mv "$HOME/.nexus/agents/tech/sub-agents/$sub/SOUL.md" \
       "$HOME/.nexus/prompt-library/souls/$sub.md"
    ln -s "$HOME/.nexus/prompt-library/souls/$sub.md" \
       "$HOME/.nexus/agents/tech/sub-agents/$sub/SOUL.md"
    track_symlink "$HOME/.nexus/agents/tech/sub-agents/$sub/SOUL.md"
    echo "  ✓ sub/$sub"
  fi
done

set +e
trap - ERR
```

**Test 4**:
```bash
# Symlinks resolve
for agent in genie tech iris mercury sentinel; do
  test -L "$HOME/.nexus/agents/$agent/SOUL.md" && \
  test -f "$HOME/.nexus/agents/$agent/SOUL.md" && \
  echo "PASS: $agent" || echo "FAIL: $agent"
done

# Smoke test hash check (re-generate hashes first)
cd ~/.nexus && bash scripts/rebuild-registry.sh

# SOL discover funcționează
python3 -c "
from pathlib import Path
p = Path.home() / '.nexus/agents'
souls = list(p.glob('*/SOUL.md'))
print(f'Found {len(souls)} SOUL files via symlink')
assert len(souls) >= 5, f'Expected >=5, got {len(souls)}'
print('PASS')
"
```

**Gate Etapa 4**: Toate 5+ SOULs accesibile via symlink + rebuild-registry PASS → continuă.

### Etapa 5 — HIGH RISK: Bot system prompts (L8, L9 → bots/)

**Atenție**: Relay-urile citesc `system.xml` la startup. Symlink funcționează, dar trebuie testat.

```bash
# Lis system.xml — copie canonică în prompt-library, symlink în repo
cp "$HOME/repos/godagoo/claude-telegram-relay/src/prompts/system.xml" \
   "$HOME/.nexus/prompt-library/bots/lis-system.xml"
# Înlocuiește cu symlink
mv "$HOME/repos/godagoo/claude-telegram-relay/src/prompts/system.xml" \
   "$HOME/repos/godagoo/claude-telegram-relay/src/prompts/system.xml.bak"
ln -s "$HOME/.nexus/prompt-library/bots/lis-system.xml" \
   "$HOME/repos/godagoo/claude-telegram-relay/src/prompts/system.xml"
track_symlink "$HOME/repos/godagoo/claude-telegram-relay/src/prompts/system.xml"

# Kara (dacă există)
if [ -f "$HOME/kara-telegram-relay/src/prompts/system.xml" ]; then
  cp "$HOME/kara-telegram-relay/src/prompts/system.xml" \
     "$HOME/.nexus/prompt-library/bots/kara-system.xml"
  mv "$HOME/kara-telegram-relay/src/prompts/system.xml" \
     "$HOME/kara-telegram-relay/src/prompts/system.xml.bak"
  ln -s "$HOME/.nexus/prompt-library/bots/kara-system.xml" \
     "$HOME/kara-telegram-relay/src/prompts/system.xml"
  track_symlink "$HOME/kara-telegram-relay/src/prompts/system.xml"
fi
```

**Test 5**:
```bash
# Symlink resolve
test -L "$HOME/repos/godagoo/claude-telegram-relay/src/prompts/system.xml" && \
test -f "$HOME/repos/godagoo/claude-telegram-relay/src/prompts/system.xml" && \
echo "PASS: Lis symlink" || echo "FAIL: Lis symlink"

# Content check
head -5 "$HOME/repos/godagoo/claude-telegram-relay/src/prompts/system.xml"
# Așteptat: XML valid

# Relay restart test (opțional — Pafi decide)
# launchctl kickstart -k gui/$(id -u)/com.claude.telegram-relay
```

**Gate Etapa 5**: Symlinks resolve + XML valid → continuă. Relay restart = decizie Pafi.

**⚠️ Git Safety (F3)**: Symlinks în relay repos (L8, L9) vor apărea în `git status`.
- **NU** commit-ui symlink-urile — `git checkout -- src/prompts/system.xml` le va suprascrie
- Adaugă în `.gitignore` al fiecărui relay repo: `src/prompts/system.xml` (opțional, discută cu Pafi)
- Alternativă: păstrează `system.xml.bak` ca safety net pentru `git checkout`

**⚠️ Sync Safety (FN3)**: `sync-memory.sh` (git auto-sync, 1min) sincronizează `memory/` via git.
- Git stochează symlinks ca fișiere text cu target path-ul → pe alt clone, symlink-ul rezolvă doar dacă `prompt-library/` există la aceeași cale
- `rsync` fără `--copy-links` copiază symlink-uri, nu conținut — verifică `sync-memory.sh` flags
- **Recomandare**: adaugă `--copy-links` în rsync (dacă folosit) SAU acceptă că sync-ul propagă symlinks

### Etapa 6 — FINALIZARE: Index + Update referințe

**6a. Generează INDEX.md**
```bash
# Generează INDEX.md cu valori evaluate
cat > ~/.nexus/prompt-library/INDEX.md << INDEXEOF
# Prompt Library Index
**Generated**: $(date +%Y-%m-%d)
**Total files**: $(find ~/.nexus/prompt-library -name "*.md" -o -name "*.xml" | wc -l | tr -d ' ')

## Structura
| Folder | Conținut | Fișiere | Sursă originală |
|--------|---------|---------|-----------------|
| agents/ | Agent classification prompts | 12 | memory/agent-prompts/ (symlink) |
| souls/ | NexusOS agent SOUL files | 9 | agents/*/SOUL.md (symlinks) |
| openclaw/ | OpenClaw soul backups | 8 | memory/openclaw-configs/souls/ (symlinks) |
| bots/ | Bot system prompts | 3-4 | relay repos + memory (symlinks) |
| techniques/ | PE training procedures | 116 | procedures/training/ai-prompt-engineering/ (symlink) |
| methodology/ | PromptForge docs | 3 | memory/promptforge*.md (symlinks) |
| patterns/ | P1-P69 pattern library | varies | Created in-place |
INDEXEOF
```

**6b. Update SOL discover.py** — fix sub-agent glob:
- Linia cu `scan_dir.glob("*/SOUL.md")` → adaugă și `scan_dir.glob("tech/sub-agents/*/SOUL.md")`
- SAU schimbă în `scan_dir.glob("**/SOUL.md")` (mai robust)

**6c. Re-run discover.py + rebuild-registry.sh** → regenerează manifest.json, queue.json, soul-hashes.json cu noile path-uri (care vor fi symlink-resolved la originale în prompt-library/).

**6d. Update documentație** (best-effort, non-blocking):
- MEMORY.md: adaugă referință la prompt-library
- SOL procedure: update scan paths note

### Etapa 7 — ROLLBACK PLAN (dacă orice etapă fail)

```bash
BACKUP_FILE="$HOME/.nexus/backups/prompt-library-pre-consolidation-*.tar.gz"
LATEST_BACKUP=$(ls -t $BACKUP_FILE | head -1)

# Șterge DOAR symlinks create de această procedură (din manifest)
MANIFEST="$HOME/.nexus/prompt-library/.symlink-manifest"
if [ -f "$MANIFEST" ]; then
  while IFS= read -r link; do
    [ -L "$link" ] && rm "$link"
  done < "$MANIFEST"
fi

# Restaurează din backup
cd / && tar xzf "$LATEST_BACKUP"

# Verificare
ls ~/.claude/projects/-Users-pafi/memory/agent-prompts/*.md | wc -l
# Așteptat: 12

# Re-run discover + rebuild
cd ~/.nexus && python3 scripts/prompt-optimizer-discover.py
cd ~/.nexus && bash scripts/rebuild-registry.sh
```

---

## 6. Ce NU mutăm (și de ce)

| Item | Motiv | Plan viitor |
|------|-------|-------------|
| 22 scripturi Python cu prompturi embedded | Tightly coupled — necesită refactoring (extract prompt → load from file) | Proiect separat: "Script Prompt Externalization" |
| OpenClaw VPS SOUL files | Hardcoded paths în gateway configs, alt server | Doar backup-urile locale se mută |
| Cortex 620+ prompturi | Deja centralizate în Cortex | N/A |
| CLAUDE.md embedded sections | Nu e prompt, e metadata/instructions | N/A |

---

## 7. Verificare finală (post-consolidation)

- [ ] `ls -la ~/.nexus/prompt-library/` — 7 subdirectoare
- [ ] `find ~/.nexus/prompt-library -name "*.md" -o -name "*.xml" | wc -l` — ≥150 fișiere
- [ ] Toate symlinks resolve: `find ~/.nexus/agents ~/.claude/projects/-Users-pafi/memory -type l -exec test -e {} \; -print | wc -l` = count of created symlinks
- [ ] `python3 ~/.nexus/scripts/prompt-optimizer-discover.py` → 22+ prompts discovered
- [ ] `cat ~/.claude/projects/-Users-pafi/memory/promptforge.md | head -1` → "## PromptForge v3.7"
- [ ] `cat ~/.nexus/agents/genie/SOUL.md | head -1` → conținut valid
- [ ] `cat ~/.nexus/procedures/training/ai-prompt-engineering/C-042-persona-pattern.md | head -1` → conținut valid
- [ ] Manifest paths valid:
  ```bash
  python3 -c "
  import json
  m = json.load(open('$HOME/.nexus/optimization/manifest.json'))
  broken = [p['id'] for p in m['prompts'] if not __import__('os').path.exists(p['path'])]
  print(f'Broken paths: {len(broken)}')
  for b in broken: print(f'  ✗ {b}')
  assert len(broken) == 0, 'FAIL: broken paths in manifest'
  print('PASS')
  "
  ```
- [ ] Relay test (opțional): Lis răspunde pe Telegram
- [ ] Symlink manifest populated: `wc -l ~/.nexus/prompt-library/.symlink-manifest` ≥ 9 entries

---

## 8. Enforcement Loop

### WHERE
- SOL weekly scan (Sunday 22:00): verifică că toate symlinks sunt intacte
- Session start Step 7: dacă `~/.nexus/prompt-library/INDEX.md` lipsește → warn

### WHEN
- La orice adăugare de prompt/SOUL nou → adaugă în `prompt-library/`, creează symlink
- La orice FORGE-AUDIT pe prompts → referă `prompt-library/` ca sursa de adevăr

### CONNECT
- **SOL v1.5**: scan paths include `prompt-library/` direct
- **FORGE-AUDIT**: audit targets pot fi `prompt-library/{category}/{file}`
- **PromptForge v3.7**: F7 Library save referă `prompt-library/methodology/`

---

## Checklist Pre-Publicare

- [x] Backup plan documentat (Etapa 0)
- [x] Rollback plan documentat (Etapa 7)
- [x] Dependency audit complet (§4)
- [x] Fiecare etapă are test gates
- [x] Excluderi documentate cu motivul (§6)
- [x] Enforcement loop definit (§8)

---

## Changelog

| Versiune | Data | Modificări |
|----------|------|-----------|
| 1.0 | 2026-03-19 | Versiune inițială. Inventar 10 locații, plan 7 etape, dependency audit pe 9 consumatori. |
| **1.1** | **2026-03-19** | **FORGE-AUDIT fixes (10 findings)**: F1 rollback symlink manifest (track_symlink helper, 9 call sites), F2 heredoc interpolation, F3 git safety warning, F4 step 2d for-loop instead of glob, F5 hidden files cleanup before rmdir, F6 atomic guard (set -e + trap ERR) pe Etapa 4, F8 manifest verification Python, FN1 track_symlink wired into all ln -s calls, FN2 openclaw per-file symlinks, FN3 sync-memory.sh symlink safety note. Score: 2.75→3.25+ CONDITIONAL→PASS. |
