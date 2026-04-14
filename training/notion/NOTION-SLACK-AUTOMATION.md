---
type: procedure
created: 2026-03-22
status: active
slug: notion-slack-automation
tags: [procedure, nexus]
---

# NOTION-SLACK-AUTOMATION — Procedură Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-05
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Configurarea automatizărilor native Notion-Slack pentru notificări în timp real când proprietățile bazelor de date se schimbă, fără a necesita Zapier.

---

## §1 Problema

Fără notificări automate, schimbările de status în bazele de date Notion sunt invizibile pentru echipă până când cineva verifică manual workspace-ul. La scară (297+ proceduri, proiecte active, task-uri), urmărirea manuală a statusului este imposibilă și generează blocaje. Notion are o integrare nativă Slack care elimină Zapier pentru cazul cel mai frecvent: "notifică echipa când status-ul se schimbă".

Situații acoperite:
- Ai nevoie ca echipa să fie notificată în Slack când un task/procedură/proiect schimbă status
- Vrei să elimini verificările manuale periodice ale bazei de date
- Construiești un sistem de alertă pentru priorități critice (Priority = High → #alerts)
- Ai nevoie de notificări personale Slack pentru @mention-uri din Notion

---

## §2 Procedura

### Pas 1: Adaugă integrarea Slack la nivel de workspace Notion
1. Deschide Notion → Settings (engrenaj în sidebar sau Cmd+,)
2. Navighează la: **Settings → My Connections**
3. Caută "Slack" în lista de integrări disponibile
4. Click "Connect" → se deschide fereastra de autorizare Slack
5. Selectează workspace-ul Slack corect din dropdown-ul de sus
6. Click "Allow" → Notion primește permisiunea de a posta în Slack

### Pas 2: Configurează notificările personale Slack
Acest pas activează notificările Slack pentru @mention-uri personale:
1. Settings → **My Notifications**
2. Secțiunea "Slack Notifications" → selectează workspace-ul Slack conectat
3. Toggle "Notify me in Slack" → ON
4. Acum: oricine te menționează cu `@NumeleTău` în Notion → primești DM în Slack instant

### Pas 3: Accesează editorul de automatizări din baza de date
1. Deschide baza de date pe care vrei să o automatizezi
2. Click pe iconița de setări/editare (engrenaj sau "…") din colțul dreapta sus al bazei de date
3. Click **"Automations"** din meniu
4. Click **"New Automation"**

### Pas 4: Configurează trigger-ul automatizării
În editorul de automatizare:

**Tipuri de trigger disponibile:**
- `When a property is edited` → trigger când o proprietate specifică se schimbă (cel mai folosit)
- `When a new item is added` → trigger la adăugarea unui rând nou
- `When an item is deleted` → trigger la ștergerea unui rând
- `When a date arrives` → trigger bazat pe o proprietate de tip Date

**Configurare trigger recomandat:**
- Selectează "When a property is edited"
- Alege proprietatea: "Status" (sau altă proprietate relevantă)
- Setează condiția: "is" → selectează valoarea specifică (ex: "Done", "AUDIT-PRO PASS", "Blocked")

### Pas 5: Configurează acțiunea Slack
1. Click "+ Add action" în editorul de automatizare
2. Selectează "Send Slack message"
3. Configurează:
   - **Channel**: selectează canalul Slack destinație (ex: #procedures, #alerts, #team-updates)
   - **Message**: scrie mesajul — folosește variabile dinamice pentru context relevant

**Variabile dinamice disponibile în mesaj:**
- `{{Title}}` — titlul/numele itemului care a declanșat automatizarea
- `{{Status}}` — valoarea curentă a proprietății Status
- `{{Assignee}}` — persoana asignată (dacă proprietatea Person există)
- `{{URL}}` — linkul direct la pagina din Notion
- `{{Created time}}` — când a fost creat itemul
- Orice altă proprietate din baza de date poate fi inserată ca variabilă

**Template de mesaj recomandat:**
```
✅ *{{Title}}* a trecut în status: *{{Status}}*
Asignat: {{Assignee}} | {{URL}}
```

**Template pentru alertă critică:**
```
🚨 PRIORITATE CRITICĂ: *{{Title}}* a fost marcat *{{Status}}*
Asignat: {{Assignee}} | Acțiune necesară → {{URL}}
```

### Pas 6: Adaugă automatizări multiple per bază de date (opțional)
O bază de date poate avea mai multe automatizări:
- Automatizare 1: Status → "Done" → Slack #done-channel cu mesaj de completare
- Automatizare 2: Priority → "Critical" + Status → "In Progress" → Slack #alerts cu mesaj urgent
- Automatizare 3: Status → "PENDING" → Slack DM personal cu reminder de procesare

Pentru fiecare automatizare nouă: click "+ New Automation" și repetă Pas 4-5.

### Pas 7: Testează automatizarea
1. Creează un item de test în baza de date
2. Schimbă manual proprietatea care a fost configurată ca trigger (ex: schimbă Status la "Done")
3. Verifică în Slack că mesajul a apărut în canalul corect cu variabilele populate corect
4. Dacă mesajul nu apare: verifică că integrarea Slack este activată (Pas 1) și că canalul selectat este accesibil pentru botul Notion
5. Șterge itemul de test după verificare

### Pas 8: Documentează automatizările active
Creează sau actualizează un document de referință cu toate automatizările active:
- Baza de date → Trigger → Condiție → Canal Slack → Scop
- Stochează în Cortex și în Wiki-ul workspace-ului Notion

---

## §3 Cortex Logging

Ce se salvează în Cortex după execuție:

```json
{
  "text": "Automatizare Notion-Slack configurată pe [DB-NAME]: trigger '[Proprietate] = [Valoare]' → mesaj în Slack [#canal]. Variabile folosite: [{{Var1}}, {{Var2}}]. Testat cu item real: OK.",
  "collection": "notion-operations",
  "metadata": {
    "type": "procedure-execution",
    "procedure": "NOTION-SLACK-AUTOMATION",
    "rule_id": "META-H-002",
    "tags": ["notion", "slack", "automation", "notification", "integration"]
  }
}
```

---

## §4 Enforcement Loop (META-H-002)

### WHERE
- WISH Step H — la orice moment în care Genie sau Codex configurează o automatizare Notion-Slack
- Session Checklist — verificat la sesiunile care includ setup de notificări sau integrări
- Workspace Architecture Review — audit trimestrial verifică automatizările active și funcționale

### WHEN
- La FIECARE creare de automatizare nouă Notion-Slack
- Când o automatizare existentă nu mai trimite notificări (integrare expirată sau canal șters)
- La adăugarea unui canal Slack nou care trebuie conectat la o bază de date Notion

### HOW (violation detection)
- Automatizare configurată fără testare (Pas 7) → violation
- Mesaj Slack fără variabile dinamice (mesaj static fără context) → violation (slabă valoare informațională)
- Integrare Slack configurată la nivel de cont personal (My Connections) dar notificările personale (Pas 2) nu sunt activate → violation parțială
- Automatizări active nedocumentate (Pas 8 sărit) → violation
- Runner: audit manual la AUDIT-PRO periodic; verificare funcționalitate la session start dacă automatizările sunt critice

### CONNECT
- META-H-002 → enforcement obligatoriu
- NOTION-ZAPIER-PIPELINE-SETUP → Zapier este alternativa pentru cazuri mai complexe sau tool-uri ne-native
- NOTION-DATABASE-DESIGN-PATTERN → baza de date trebuie să aibă proprietatea Status înainte de configurarea automatizării
- `procedure-health.json` → entry: `NOTION-SLACK-AUTOMATION`

### VERIFY (procedural checkpoint)
La finalul execuției procedurii, agentul care a executat-o verifică:
- [ ] Procedura a fost executată complet? (toți pașii §1-§8 urmați)
- [ ] Automatizarea a trimis un mesaj real în Slack la testare?
- [ ] VK emis în sesiune?
- [ ] Dacă oricare = NU → procedura NU e completă, nu marca DONE

**Două VK-uri obligatorii per execuție** (per VK-H-001):
1. **Execution VK**: `✅ [PROC] NOTION-SLACK-AUTOMATION | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. **Save VK**: `✅ [CORTEX] "NOTION-SLACK-AUTOMATION" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model | Motivul |
|-----------|-------|---------|
| Configurare automatizare standard | Sonnet (agent activ) | Pași structurați, UI Notion |
| Design sistem de notificări complex (multiple DB, conditional logic) | Opus | Arhitectură de alerting |
| Debugging automatizare care nu se declanșează | Sonnet | Troubleshooting procedural |
| Audit automatizări active la nivel de workspace | Opus single agent | Review comprehensiv |

---

## §5 Dependențe

| Componentă | Rol | Path/Endpoint |
|-----------|-----|---------------|
| NOTION-DATABASE-DESIGN-PATTERN | Baza de date sursă trebuie să existe cu proprietatea Status | `~/.nexus/procedures/training/notion/NOTION-DATABASE-DESIGN-PATTERN.md` |
| NOTION-ZAPIER-PIPELINE-SETUP | Alternativă pentru automatizări mai complexe sau tool-uri ne-native | `~/.nexus/procedures/training/notion/NOTION-ZAPIER-PIPELINE-SETUP.md` |
| Slack Workspace | Destinația notificărilor | Workspace activ Slack |
| Notion Web App | Editorul de automatizări | `https://notion.so` |
| notion.com/integrations | Pagina de management integrări | `https://notion.com/integrations` |
| procedure-health.json | Registry de sănătate proceduri | `~/.nexus/procedures/openclaw/procedure-health.json` |
