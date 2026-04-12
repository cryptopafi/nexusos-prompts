---
id: ND-008
title: "Constrained and Guided Generation"
domain: ai-prompt-engineering
source: "NirDiamant Prompt Engineering Guide — https://github.com/NirDiamant/Prompt_Engineering"
version: 1.0
forge_score: 3.5
business_mapping: ["all"]
---

# ND-008: Constrained and Guided Generation

## Obiectiv

Controlarea output-ului LLM prin constrângeri explicite: format, lungime, vocabular, structură, stil. Transformă output-ul open-ended în output predictibil și parsabil. Esențial pentru integrare în pipelines.

## Pași

### Pas 1: Constrângeri de format
```
Extract the following fields from this invoice and return as JSON:

{
  "vendor_name": "",
  "invoice_date": "YYYY-MM-DD",
  "total_amount": 0.00,
  "currency": "",
  "line_items": [{"description": "", "amount": 0.00}]
}

Invoice text: {INVOICE_TEXT}
```

### Pas 2: Constrângeri de lungime
```
Summarize this article.

Constraints:
- Exactly 3 sentences
- First sentence: main finding
- Second sentence: methodology
- Third sentence: implications

Article: {TEXT}
```

### Pas 3: Constrângeri de vocabular/stil
```
Explain quantum computing to a 10-year-old.

Rules:
- No technical jargon
- Use analogies from everyday life
- Maximum 100 words
- Must include at least one comparison to something the child knows
```

### Pas 4: Constrângeri structurale (output parsing)
```
Analyze this code for bugs. Respond in EXACTLY this format:

BUG_COUNT: [number]
SEVERITY: [critical/high/medium/low]
BUGS:
1. Line [N]: [description]
2. Line [N]: [description]
FIX_SUGGESTION: [brief suggestion]

Code:
{CODE}
```

### Pas 5: Constrângeri negative (ce să NU facă)
```
Write a product description.

DO NOT:
- Use superlatives (best, greatest, amazing)
- Make unverified claims
- Use more than 3 sentences
- Include pricing information

DO:
- Focus on specific features
- Use measurable specifications
- Include one user benefit per feature
```

### Pas 6: Validarea output-ului constrainat
```python
def validate_output(output, constraints):
    checks = {
        "max_words": len(output.split()) <= constraints.get("max_words", float('inf')),
        "json_valid": is_valid_json(output) if constraints.get("json") else True,
        "no_forbidden": not any(w in output.lower() for w in constraints.get("forbidden_words", [])),
    }
    return all(checks.values()), checks
```

## Verificare

- [ ] Constrângerile sunt explicite în prompt (nu implicite)
- [ ] Formatul de output e specificat exact (cu exemplu dacă complex)
- [ ] Constrângeri negative incluse (ce să NU facă)
- [ ] Output-ul validat programmatic (dacă parsabil)
- [ ] Testat pe 5+ input-uri

## Instrumente

| Model | Note |
|-------|------|
| Claude | Excelent la respectarea constrângerilor |
| GPT-4/5 + JSON mode | JSON output garantat |
| Pydantic + Instructor | Validare structurală automată |

## Note

- **Când să folosești**: API pipelines; output parsabil; integrare cu alte sisteme; consistență output.
- **Când NU**: Explorare creativă; brainstorming (constrângerile limitează creativitatea).
- **Greșeli comune**: Constrângeri contradictorii; prea multe constrângeri (modelul "uită"); lipsa validării.
- **Relație**: ND-017 (formatting), ND-015 (length management), ND-016 (negative prompting).
