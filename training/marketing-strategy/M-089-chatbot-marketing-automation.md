---
type: procedure
created: 2026-03-22
status: active
slug: m-089-chatbot-marketing-automation
tags: [procedure, nexus]
---

# Chatbot Marketing Automation (ManyChat) — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Proiectarea, implementarea și optimizarea fluxurilor de chatbot marketing pe ManyChat pentru automatizarea nurturing-ului, calificării lead-urilor și creșterii conversiei.

---

## 1. Problema

Fără automatizare prin chatbot, follow-up-ul cu lead-urile de pe social media este lent, inconsistent și dependentă de resursa umană disponibilă. ManyChat permite automatizarea conversațiilor personalizate la scară, dar fluxurile prost proiectate creează experiențe frustrante care deteriorează brandul și reduc conversia.

Situații acoperite:
- Construirea unui flux de chatbot pentru calificarea și nurturing-ul lead-urilor de pe Instagram/Facebook
- Automatizarea campaniei de DM pentru un launch sau promoție specifică
- Optimizarea unui flux existent cu rată scăzută de conversie sau engagement

---

## 2. Procedura

### Pas 1: Definirea obiectivului și a conversion goal
Stabilește un singur obiectiv primar pentru fluxul de chatbot: calificare lead, vânzare directă, book call, descărcare lead magnet, sau nurturing. Un flux = un obiectiv. Definește conversion event-ul măsurabil (link click, răspuns la keyword, completare formular). Documentează audiența țintă și contextul de intrare în flux (DM trigger, comment keyword, link click).

### Pas 2: Maparea conversației și a arborelui de decizii
Creează flow chart-ul complet al conversației înainte de implementare: mesaj inițial → ramuri de răspuns → noduri de decizie → ieșiri (conversie, opt-out, escalare la uman). Fiecare nod trebuie să aibă o cale de ieșire clară, inclusiv pentru răspunsuri neașteptate. Limita: maximum 5 nivele de profunzime pentru a evita abandonul.

### Pas 3: Redactarea mesajelor de chatbot
Scrie toate mesajele respectând vocea brandului în format conversațional (nu corporate). Mesajele scurte (max 100 cuvinte per bublă), cu întrebări simple cu 2-3 variante de răspuns predefinite (quick replies). Primul mesaj: personalizat cu firstname, clar despre ce urmează și ușor de răspuns. Include opt-out vizibil și ușor accesibil în primele 2 mesaje.

### Pas 4: Configurarea tehnică în ManyChat
Creează fluxul în ManyChat: setează trigger-ele (keywords, Growth Tools, Entry Points), construiește flow-ul cu blocurile corespunzătoare (Message, Action, Condition, Smart Delay). Configurează tag-urile și câmpurile custom pentru segmentare. Setează integrarea cu CRM sau email platform pentru transferul lead-urilor calificate. Testează integral fluxul cu un cont de test înainte de lansare.

### Pas 5: Compliance și setări de privacy
Verifică conformitatea cu politicile Meta: opt-in explicit documentat, mesaje în fereastra de 24h sau cu tag-uri aprobate, fără spam. Adaugă mesajele de unsubscribe și procesează cererile automat. Documentează baza legală pentru contactare (GDPR dacă aplicabil). Fără complianță, fluxul nu se lansează.

### Pas 6: Lansare, monitorizare și optimizare
Lansează fluxul pentru un segment mic inițial (10-20% din audiență) pentru a valida funcționalitatea. Monitorizează primele 48h: rata de deschidere, rata de click pe quick replies, punctele de abandon, conversii. Identifica nodurile cu abandon ridicat și optimizează mesajele sau simplifica alegerile. Scale la 100% după validare.

### Pas 7: Raportare, iterare și maintenance
Setează raportare săptămânală automată: open rate, CTR, opt-out rate, conversie per flux. Revizuiește fluxurile lunar pentru relevanță și performance. Arhivează fluxurile inactive. Documentează learningurile în Cortex și actualizează procedure-health.json.

---

## 3. Cortex Logging

```json
{
  "text": "Flux chatbot ManyChat proiectat și implementat: obiectiv definit, arboret decizii mapat, mesaje redactate, compliance verificat, lansare validată.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "M-089",
    "domain": "marketing-strategy",
    "rule_id": "META-H-002",
    "tags": ["chatbot", "manychat", "marketing-automation", "messenger", "instagram-dm", "lead-nurturing"],
    "has_enforcement_loop": true,
    "forge_version": "1.3"
  }
}
```

---

## 4. Enforcement Loop

### WHERE
WISH Step H + Training Mode

### WHEN
La fiecare execuție

### HOW (violation detection)
- Procedura executată fără toți pașii din §2 → violation
- Output fără VK → violation
- Flux lansat fără testare completă pe cont de test → violation
- Flux lansat fără verificare compliance Meta și GDPR → violation
- Flux cu mai mult de un obiectiv primar → violation (diluare focus)
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum marketing-strategy

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Compliance Meta și GDPR verificat documentat?
- [ ] Flux testat integral pe cont de test înainte de lansare?

**VK-uri obligatorii:**
1. `✅ [PROC] M-089 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Chatbot Marketing Automation (ManyChat)" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| ManyChat | Platform de chatbot și automatizare DM |
| Meta Business Suite | Configurarea trigger-elor și Growth Tools |
| CRM (HubSpot/Pipedrive) | Transferul lead-urilor calificate |
| Email platform | Integrare pentru nurturing post-chatbot |
| procedure-health.json | Tracking execuție procedură |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Open rate flux chatbot | ≥70% |
| Rată conversie la obiectiv | ≥15% |
| Opt-out rate | ≤5% |
