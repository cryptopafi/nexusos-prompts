---
id: PR-006
title: "Exemplar Format"
domain: ai-prompt-engineering
source: "The Prompt Report — https://arxiv.org/html/2406.06608"
version: 1.0
forge_score: 3.5
business_mapping: ["all"]
---

# PR-006: Exemplar Format

## Obiectiv

Formatul exemplelor (cum sunt structurate vizual) afectează performanța. "Q: {input}, A: {label}" e cel mai comun, dar formatul optimal variază per task. Această procedură ajută la selectarea și testarea formatului potrivit.

## Pași

### Pas 1: Alege un format de bază din cele standard

**Format A — Q/A (clasic):**
```
Q: What is the capital of France?
A: Paris

Q: What is the capital of Japan?
A: Tokyo

Q: What is the capital of {COUNTRY}?
A:
```

**Format B — Input/Output (tehnic):**
```
Input: "2024-03-15"
Output: "March 15, 2024"

Input: "2023-12-01"
Output: "December 1, 2023"

Input: "{DATE}"
Output:
```

**Format C — Labeled sections (complex):**
```
Text: "The quarterly revenue increased by 15% year-over-year."
Category: Financial
Sentiment: Positive
Key metric: 15% revenue growth

Text: "{NEW_TEXT}"
Category:
Sentiment:
Key metric:
```

**Format D — Markdown/Structured (multi-field):**
```
### Example 1
- **Email**: "Meeting rescheduled to 4pm"
- **Priority**: Low
- **Action**: Update calendar

### Your task
- **Email**: "{NEW_EMAIL}"
- **Priority**:
- **Action**:
```

### Pas 2: Potrivește formatul cu output-ul dorit
Dacă vrei output structurat (JSON, tabel), exemplele trebuie să fie structurate. Dacă vrei text liber, folosește Q/A simplu.

### Pas 3: Asigură consistență perfectă între exemple
Fiecare exemplu trebuie să aibă EXACT aceeași structură. Orice variație va confuza modelul.

### Pas 4: Testează 2-3 formate
Rulează același task cu formate diferite. Compară calitatea output-ului.

### Pas 5: Folosește delimitatori clari
Separatoarele între exemple trebuie vizibile: linii goale, `---`, sau `###`. Fără separatoare, modelul poate confunda un exemplu cu altul.

### Pas 6: Aliniază formatul exemplelor cu output-ul dorit
Dacă vrei JSON output, exemplele trebuie să arate JSON output. Modelul va mima exact structura exemplelor.

## Verificare

- [ ] Toate exemplele au format identic
- [ ] Delimitatori clari între exemple
- [ ] Formatul exemplelor = formatul output-ului dorit
- [ ] Testat minim 2 formate diferite
- [ ] Formatul ales documentat pentru refolosire

## Instrumente

| Model | Note |
|-------|------|
| Claude | Excelent cu Format C și D (structurat) |
| GPT-4/5 | Excelent cu toate formatele |
| Modele mici | Preferă Format A (simplu Q/A) |

## Note

- **Când să folosești**: La orice Few-Shot prompt, mai ales când output-ul are structură specifică.
- **Când NU**: Zero-shot; task-uri unde formatul nu contează.
- **Greșeli comune**: Formate inconsistente între exemple; format de exemplu diferit de output dorit; lipsa delimitatorilor.
- **Relație**: PR-001 (Few-Shot base), PR-017 (prompt formatting general).
