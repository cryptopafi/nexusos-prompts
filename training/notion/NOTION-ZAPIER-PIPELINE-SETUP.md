---
type: procedure
created: 2026-03-22
status: active
slug: notion-zapier-pipeline-setup
tags: [procedure, nexus]
---

# NOTION-ZAPIER-PIPELINE-SETUP — Procedură Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-05
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Conectarea Notion la aplicații externe prin Zapier pentru automatizarea fluxurilor de date, inclusiv autorizare, configurare trigger/action, și testare pipeline.

---

## §1 Problema

Notion are integrări native cu ~200 de tool-uri. Restul ecosistemului (Gmail, Google Forms, ChatGPT, Slack, Airtable, Typeform, și alte 7000+ aplicații) necesită Zapier ca middleware. Fără un proces structurat, zap-urile sunt configurate greșit, autorizările expiră, și pipelinele eșuează silențios — datele par că intră dar nu apar în Notion.

Situații acoperite:
- Ai nevoie de automatizare între Notion și un tool care nu are integrare nativă Notion
- Vrei să capturezi automat date din Gmail, Google Forms, sau alte surse în Notion
- Construiești un pipeline Notion → ChatGPT → Notion pentru procesare AI a conținutului
- Automatizezi notificări sau acțiuni declanșate de schimbări în Notion

---

## §2 Procedura

### Pas 1: Creează contul Zapier și autorizează Notion
1. Accesează `zapier.com` → Sign Up (recomandă email de business, nu personal)
2. În dashboard, click "Create" → "New Zap"
3. La prima conectare Notion: click "Sign In" → se deschide fereastra de autorizare Notion
4. **CRITIC — selectare pagini**: în dialogul Notion, selectează **"All pages"** (nu pages individuale)
   - Dacă selectezi pages individuale, Zapier nu va putea accesa baze de date create ulterior
5. Click "Allow access" → conexiunea este salvată

### Pas 2: Alege trigger-ul (evenimentul care pornește zap-ul)
**Trigger-urile disponibile când Notion este sursa:**
- `New Database Item` — un rând nou este adăugat în baza de date selectată
- `Database Item Edited` — orice proprietate a unui rând existent se schimbă
- `Page Updated` — orice pagină Notion este actualizată (mai larg decât DB)

**Trigger-urile din aplicații externe care scriu în Notion (cele mai frecvente):**
- Gmail: `New Email Matching Search` (email care îndeplinește criterii specifice)
- Google Forms: `New Form Response`
- Typeform: `New Entry`
- Slack: `New Message Posted to Channel`
- Webhook: `Catch Hook` (orice sursă cu webhook support)

Selectează trigger-ul → configurează: alege baza de date sau contul sursă → testează trigger-ul cu date reale (butonul "Test trigger").

### Pas 3: Configurează action-ul (ce se întâmplă după trigger)
**Action-urile disponibile când Notion este destinația:**
- `Create Database Item` — creează un rând nou cu câmpurile mapate
- `Update Database Item` — actualizează un rând existent (necesită un ID unic pentru identificare)
- `Find Database Item` — caută un rând (folosit ca pas intermediar înainte de update)
- `Create Page` — creează o pagină Notion (nu un rând de DB)

**Maparea câmpurilor (field mapping):**
- Câmpul Zapier din trigger → Câmpul Notion din baza de date
- Exemplu Gmail → Notion: Subject → Title, From → Owner, Body → Notes, Date → Created Date
- Folosește variabilele dinamice Zapier (icon cu "+" albastru) pentru a mapa câmpuri de la trigger

### Pas 4: Template-uri esențiale pentru Nexus

**Template A — Gmail → Notion (capturare email-uri importante):**
1. Trigger: Gmail → New Email Matching Search (ex: `from:client@domain.com subject:brief`)
2. Action: Notion → Create Database Item în Projects DB
3. Map: Subject → Title, From → Client, Date → Received Date, Body → Description

**Template B — Google Forms → Notion → Slack (feedback pipeline):**
1. Trigger: Google Forms → New Form Response
2. Action 1: Notion → Create Database Item (toate câmpurile formularului → baza de date)
3. Action 2: Slack → Send Message în #feedback cu link Notion + câmpuri cheie

**Template C — Notion → ChatGPT → Notion (procesare AI):**
1. Trigger: Notion → New Database Item (o idee adăugată în Ideas DB)
2. Action 1: ChatGPT → Send Message cu promptul: "Develop this idea into a full brief: {{Idea Text}}"
3. Action 2: Notion → Update Database Item — scrie răspunsul ChatGPT în câmpul "AI Brief"

**Template D — Zapier AI (generare automată outline zap):**
- În loc să configurezi manual: pe pagina de creare zap, click "Describe what your Zap should do"
- Descrie în text natural: "When a new email from a client arrives in Gmail with subject containing 'brief', create a new item in my Notion Projects database"
- Zapier AI generează structura zap-ului — revizuiește și confirmă

### Pas 5: Testează zap-ul cu date reale
1. Click "Test" pe fiecare pas — Zapier execută pasul cu date reale din trigger
2. Verifică în Notion că itemul a fost creat corect (toate câmpurile mapate corect)
3. Testează cu date edge-case: email fără subject, formular cu câmpuri opționale goale
4. Verifică că erorile sunt captate (Zapier trimite email de notificare când un zap eșuează)

### Pas 6: Publică și monitorizează
1. Toggle-ul "Zap is OFF" → click pentru a activa → "Zap is ON"
2. Monitorizează primele 24h în Zapier Dashboard → "Task History"
3. Setează notificările Zapier pentru zap-uri care eșuează (Settings → Notifications)
4. Documentează zap-ul: adaugă o descriere în câmpul "Zap Name" (format: `[SURSĂ] → [DESTINAȚIE]: [ce face]`)

---

## §3 Cortex Logging

Ce se salvează în Cortex după execuție:

```json
{
  "text": "Zap Zapier creat: [SURSĂ] → Notion [DB-NAME]. Trigger: [trigger type]. Action: [action type]. Pipeline: [descriere scurtă ce automatizează]. Status: ACTIVE. Testat cu date reale: OK.",
  "collection": "notion-operations",
  "metadata": {
    "type": "procedure-execution",
    "procedure": "NOTION-ZAPIER-PIPELINE-SETUP",
    "rule_id": "META-H-002",
    "tags": ["notion", "zapier", "automation", "integration", "pipeline"]
  }
}
```

---

## §4 Enforcement Loop (META-H-002)

### WHERE
- WISH Step H — la orice moment în care Genie sau Codex conectează Notion la un tool extern via Zapier
- Session Checklist — verificat la sesiunile care includ setup de automatizări
- Workspace Architecture Review — la audit trimestrial de workspace

### WHEN
- La FIECARE creare de zap nou care implică Notion
- Când un zap existent eșuează sau produce date incorecte în Notion
- La expirarea autorizării Notion în Zapier (reautorizare necesară)

### HOW (violation detection)
- Zapier autorizat cu "Select pages" în loc de "All pages" → violation (baze de date noi nu vor fi accesibile)
- Zap publicat fără testare cu date reale (Pas 5) → violation
- Zap fără nume descriptiv (format `[SURSĂ] → [DESTINAȚIE]: [ce face]`) → violation
- Zap activ fără monitorizare Task History în primele 24h → violation
- Runner: verificare manuală la AUDIT-PRO; Zapier trimite notificări automate la eșecuri

### CONNECT
- META-H-002 → enforcement obligatoriu
- NOTION-SLACK-AUTOMATION → Template B din Pas 4 include Slack ca action
- NOTION-DATABASE-DESIGN-PATTERN → baza de date destinație trebuie să existe înainte de configurare
- `procedure-health.json` → entry: `NOTION-ZAPIER-PIPELINE-SETUP`

### VERIFY (procedural checkpoint)
La finalul execuției procedurii, agentul care a executat-o verifică:
- [ ] Procedura a fost executată complet? (toți pașii §1-§6 urmați)
- [ ] Zap-ul este activ și a procesat cel puțin un item real cu succes?
- [ ] VK emis în sesiune?
- [ ] Dacă oricare = NU → procedura NU e completă, nu marca DONE

**Două VK-uri obligatorii per execuție** (per VK-H-001):
1. **Execution VK**: `✅ [PROC] NOTION-ZAPIER-PIPELINE-SETUP | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. **Save VK**: `✅ [CORTEX] "NOTION-ZAPIER-PIPELINE-SETUP" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model | Motivul |
|-----------|-------|---------|
| Configurare zap standard (Template A/B) | Sonnet (agent activ) | Pași structurați, UI Zapier |
| Design pipeline complex (3+ pași, logică condițională) | Opus | Arhitectură flux de date |
| Debugging zap eșuat | Opus sau Sonnet | Depinde de complexitate |
| Generare batch zap-uri similare | Codex batch | Pattern repetitiv |

---

## §5 Dependențe

| Componentă | Rol | Path/Endpoint |
|-----------|-----|---------------|
| NOTION-DATABASE-DESIGN-PATTERN | Baza de date destinație trebuie să existe | `~/.nexus/procedures/training/notion/NOTION-DATABASE-DESIGN-PATTERN.md` |
| NOTION-SLACK-AUTOMATION | Template B include Slack ca action | `~/.nexus/procedures/training/notion/NOTION-SLACK-AUTOMATION.md` |
| Zapier (extern) | Middleware de integrare | `https://zapier.com` |
| Notion Web App | Destinația datelor automatizate | `https://notion.so` |
| ChatGPT API (extern) | Template C — procesare AI | `https://chat.openai.com` |
| procedure-health.json | Registry de sănătate proceduri | `~/.nexus/procedures/openclaw/procedure-health.json` |
