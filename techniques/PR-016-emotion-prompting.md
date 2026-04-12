---
id: PR-016
title: "Emotion Prompting"
domain: ai-prompt-engineering
source: "The Prompt Report — https://arxiv.org/html/2406.06608"
version: 1.0
forge_score: 3.5
business_mapping: ["all"]
---

# PR-016: Emotion Prompting

## Obiectiv

Emotion Prompting adaugă fraze cu relevanță psihologică în prompt (ex: "This is important to my career", "This is very important to me") care pot îmbunătăți performanța LLM-ului pe benchmarks și generare de text. Tehnica exploatează faptul că modelele au fost antrenate pe text uman unde importanța emoțională corelează cu atenția și calitatea răspunsurilor.

## Pași

### Pas 1: Identifică task-ul care beneficiază de atenție crescută
Emotion prompting funcționează cel mai bine când vrei ca modelul să fie mai atent, mai riguros sau mai creativ.

### Pas 2: Selectează stimulul emoțional potrivit

**Categoria A — Importanță personală:**
```
This is very important to my career. Please be thorough and accurate.

{ACTUAL_PROMPT}
```

**Categoria B — Consecințe:**
```
My job depends on getting this right. Double-check your reasoning.

{ACTUAL_PROMPT}
```

**Categoria C — Motivație pozitivă:**
```
I know you can do an excellent job with this. Take your time and be precise.

{ACTUAL_PROMPT}
```

**Categoria D — Urgență:**
```
This is critical and needs to be perfect. There's no room for error.

{ACTUAL_PROMPT}
```

### Pas 3: Plasează stimulul emoțional strategic
- **Înainte de task**: setează atenția ("This is important...")
- **După task**: ca reminder ("...double-check because this matters")
- **Ambele**: maximizează efectul

**Template complet:**
```
This task is extremely important for an upcoming presentation to senior leadership.

Analyze the following quarterly data and identify the top 3 trends:
{DATA}

Please be thorough — my credibility depends on the accuracy of this analysis.
```

### Pas 4: Calibrează intensitatea
- Prea puțin: niciun efect
- Optim: "This is important to me" / "Please be careful and thorough"
- Prea mult: poate cauza răspunsuri excesiv de verbose sau disclaimere excesive

### Pas 5: Combină cu alte tehnici
Emotion + Role: "As an expert, this review is critical for patient safety..."
Emotion + CoT: "Think step by step carefully — this analysis is crucial..."

### Pas 6: Evaluează impactul
Compară output-ul cu și fără stimul emoțional pe același task. Dacă nu există diferență notabilă, task-ul nu beneficiază de această tehnică.

## Verificare

- [ ] Stimulul emoțional e natural, nu manipulativ
- [ ] Intensitatea e calibrată (nu excesivă)
- [ ] Plasat strategic (înainte și/sau după task)
- [ ] Impactul evaluat vs. baseline fără emoție
- [ ] Nu produce disclaimere excesive sau verbose padding

## Instrumente

| Model | Eficacitate |
|-------|-------------|
| Claude | Moderată — răspunde la importanță dar nu dramatic |
| GPT-4 | Moderată-Bună — observabil pe task-uri complexe |
| Modele mici | Variabilă — uneori niciun efect |

## Note

- **Când să folosești**: Task-uri unde vrei atenție maximă; analize critice; review-uri de cod/text; task-uri cu consecințe reale.
- **Când NU**: Task-uri simple/factuale; când vrei răspunsuri concise (emoția tinde să facă output-ul mai lung); interacțiuni automatizate de volum mare.
- **Greșeli comune**: Stimuluri emoționale exagerate; presupunerea că funcționează pe orice task; necomparat cu baseline.
- **Relație**: PR-014 (Role Prompting), PR-015 (Style Prompting), PR-020 (RE2 — alt mod de a crește atenția).
