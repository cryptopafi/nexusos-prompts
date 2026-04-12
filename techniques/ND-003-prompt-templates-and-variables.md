---
id: ND-003
title: "Prompt Templates and Variables"
domain: ai-prompt-engineering
source: "NirDiamant Prompt Engineering Guide — https://github.com/NirDiamant/Prompt_Engineering"
version: 1.0
forge_score: 3.5
business_mapping: ["all"]
---

# ND-003: Prompt Templates and Variables

## Obiectiv

Crearea de prompt templates reutilizabile cu variabile (placeholders) care pot fi populate dinamic. Esențial pentru automatizare, consistență și scalare. Folosește Jinja2, f-strings, sau orice templating system.

## Pași

### Pas 1: Identifică părțile fixe vs. variabile ale prompt-ului
```
# Fix: instrucțiunea, formatul, rolul
# Variabil: input-ul, parametrii specifici

Fixed: "Translate the following text from {source_lang} to {target_lang}.
        Maintain the original tone and style."
Variable: source_lang, target_lang, text
```

### Pas 2: Creează template-ul cu placeholders

**Python f-string:**
```python
template = """You are a {role} with expertise in {domain}.

Analyze the following {input_type}:
{input_text}

Provide your analysis focusing on:
{focus_areas}

Format: {output_format}"""

prompt = template.format(
    role="senior financial analyst",
    domain="tech stocks",
    input_type="quarterly earnings report",
    input_text=report_text,
    focus_areas="revenue trends, profit margins, forward guidance",
    output_format="Executive summary (3 paragraphs) + bullet point recommendations"
)
```

### Pas 3: Jinja2 templates (pentru condiții și loops)
```python
from jinja2 import Template

template = Template("""
Analyze this {{ item_type }}.

{% if include_comparison %}
Compare with the previous {{ period }}'s data.
{% endif %}

Key metrics to evaluate:
{% for metric in metrics %}
- {{ metric }}
{% endfor %}

Data: {{ data }}
""")

prompt = template.render(
    item_type="sales report",
    include_comparison=True,
    period="quarter",
    metrics=["Revenue", "Customer acquisition cost", "Churn rate"],
    data=sales_data
)
```

### Pas 4: Creează o bibliotecă de templates
```python
TEMPLATES = {
    "classify": "Classify the following {input_type} into one of these categories: {categories}.\n\nInput: {input}\nCategory:",
    "summarize": "Summarize this {content_type} in {length}. Focus on {focus}.\n\n{content}",
    "translate": "Translate from {source} to {target}. Preserve {preserve}.\n\n{text}",
    "extract": "Extract {fields} from the following {source_type}:\n\n{content}\n\nOutput as JSON.",
}
```

### Pas 5: Validează variabilele
Asigură-te că toate variabilele au valori valide înainte de a trimite prompt-ul.

```python
def render_prompt(template_name, **kwargs):
    template = TEMPLATES[template_name]
    required_vars = re.findall(r'\{(\w+)\}', template)
    missing = [v for v in required_vars if v not in kwargs]
    if missing:
        raise ValueError(f"Missing variables: {missing}")
    return template.format(**kwargs)
```

### Pas 6: Versionează templates
Salvează template-urile cu versiune pentru tracking:
```python
TEMPLATE_REGISTRY = {
    "classify_v1.0": {"template": "...", "accuracy": 0.92, "model": "claude-sonnet"},
    "classify_v1.1": {"template": "...", "accuracy": 0.95, "model": "claude-sonnet"},
}
```

## Verificare

- [ ] Template-ul separă clar fix de variabil
- [ ] Toate variabilele au valori default sau validare
- [ ] Template-ul e testat cu minim 3 seturi de variabile
- [ ] Salvat în bibliotecă cu versiune
- [ ] Documentat: ce variabile, ce valori acceptabile

## Instrumente

| Instrument | Rol |
|-----------|-----|
| Jinja2 | Templating avansat (loops, conditionals) |
| Python f-strings | Templating simplu |
| LangChain PromptTemplate | Framework dedicat |

## Note

- **Când să folosești**: Orice prompt folosit de >2 ori; automatizare; API pipelines; consistență cross-team.
- **Când NU**: One-off prompts; explorare/prototipare.
- **Greșeli comune**: Variables nesanitizate (injection risk); template prea rigid; lipsa versionării.
- **Relație**: ND-001 (fundamentals), ND-011 (chaining), ND-017 (formatting).
