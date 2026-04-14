---
type: procedure
created: 2026-03-22
status: active
slug: c-081-ipo-due-diligence-sec
tags: [procedure, nexus]
---

# IPO Due Diligence Using SEC Filings — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Proces structurat de analiză a documentelor SEC (S-1, S-11, prospectus) pentru evaluarea IPO-urilor înainte de decizia de participare.

> **Disclaimer**: Analiza SEC filings este educativă. Nu constituie recomandare de investiție.

---

## 1. Problema

IPO-urile sunt prezentate publicului cu o narativă optimistă construită de bănci de investiții cu interes direct în pricing ridicat. Fără citirea sistematică a documentelor SEC oficiale, investitorii retail acționează pe baza buzz-ului media în loc de informații verificate din sursa primară.

Situații acoperite:
- Companie populară în media care intră la bursă cu valuare ridicată față de fundamentale
- IPO cu structuri de acțiuni duale sau control intern concentrat (insider ownership ridicat)
- Evaluarea perioadei de lock-up și riscul de presiune vânzare post-expirare

---

## 2. Procedura

### Pas 1: Descarcă și citește S-1/S-11 de pe SEC EDGAR
Accesează SEC EDGAR (edgar.sec.gov), caută compania după nume, descarcă cel mai recent S-1 sau S-11. Documentul este sursa primară — nu citiți rezumate de pe site-uri financiare. Verifică data filing-ului și dacă există amendamente (S-1/A).

### Pas 2: Analizează secțiunea "Risk Factors" sistematic
Secțiunea Risk Factors este scrisă de avocați și conține riscuri reale, nu doar disclaimer standard. Caută: dependențe de un singur client (>10% din venituri), litigii pendinte, dependențe de fondatori-cheie, riscuri regulatorii specifice domeniului. Orice risc menționat explicit a apărut deja sau este probabil.

### Pas 3: Evaluează structura acționarilor și insider ownership
Caută tabelul de capitalizare (cap table): ce procent dețin fondatorii, VC-urile, angajații? Calculează ce procent din acțiuni sunt supuse lock-up (tipic 90–180 zile). La expirarea lock-up-ului există risc semnificativ de vânzare masivă. Notează data exactă de expirare.

### Pas 4: Citește "Use of Proceeds" — ce face compania cu banii
Această secțiune relevă intenția reală: banii merg spre creștere (R&D, expansiune) sau spre buyout-ul fondatorilor și VC-urilor? Dacă o proporție mare merge spre "selling shareholders" (insider liquidity), IPO-ul este parțial un exit pentru investitorii timpurii, nu finanțare de creștere.

### Pas 5: Analizează fundamentalele financiare (Financial Statements)
Caută în secțiunea financiară: rata de creștere a veniturilor (YoY), gross margin trend, cash burn rate, runway la prețul actual de finanțare. Calculează EV/Revenue și EV/EBITDA față de comparabile. Un IPO cu pierdere operatorie și burn rate mare necesită un premium de creștere justificat.

### Pas 6: Evaluează structura acțiunilor și drepturile de vot
Verifică dacă există clase de acțiuni diferite (Class A vs Class B). Fondatorii cu Class B pot deține 10x sau 20x drepturile de vot față de acțiunile publice. Aceasta înseamnă control complet al companiei chiar dacă vând majoritate din equity. Evaluează dacă ești confortabil cu această structură.

### Pas 7: Compară valuation cu comparable companies (comps)
Construiește o tabelă simplă de comps: 3–5 companii similare listate, EV/Revenue, EV/EBITDA, rata de creștere, profitabilitate. Compania din IPO trebuie să justifice premium față de comps prin creștere superioară sau moat defensibil. Dacă premium nu e justificabil, prețul IPO este agresiv.

---

## 3. Cortex Logging

```json
{
  "text": "C-081 IPO Due Diligence SEC executat: S-1 citit, risk factors analizate, cap table și lock-up evaluate, comps construite.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "C-081",
    "domain": "mba",
    "rule_id": "META-H-002",
    "tags": ["IPO", "SEC", "S-1", "due-diligence", "investing", "cap-table", "lock-up"],
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
- Analiză bazată pe rezumate media în loc de S-1 original → violation critică metodologică
- Lock-up expiry date neidentificată → violation risc management
- Comps table necompletată înainte de evaluarea valuation → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum mba

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] S-1 descărcat din SEC EDGAR (nu rezumat terț)?
- [ ] Data expirare lock-up notată explicit?

**VK-uri obligatorii:**
1. `✅ [PROC] C-081 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "IPO Due Diligence Using SEC Filings" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| SEC EDGAR (edgar.sec.gov) | Sursă primară S-1 și amendamente |
| Template comps table | Analiză comparativă valuare |
| Calculator financial metrics | EV/Revenue, burn rate, runway |
| procedure-health.json | Logging execuție procedură |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| S-1 citit integral (nu rezumat) | 100% din evaluări |
| Comps table construită per IPO analizat | 100% |
| Lock-up expiry identificat și notat | 100% |
