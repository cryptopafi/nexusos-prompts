---
id: ND-011
title: "Prompt Chaining and Sequencing"
domain: ai-prompt-engineering
source: "NirDiamant Prompt Engineering Guide — https://github.com/NirDiamant/Prompt_Engineering"
version: 1.0
forge_score: 3.5
business_mapping: ["all"]
---

# ND-011: Prompt Chaining and Sequencing

## Obiectiv

Prompt chaining conectează output-ul unui prompt ca input pentru următorul, creând pipelines de procesare. Fiecare prompt în chain e specializat pe un sub-task. Superior unui singur mega-prompt pentru task-uri complexe.

## Pași

### Pas 1: Definește chain-ul
```
Chain: Raw Text → Summarize → Extract Key Points → Generate Action Items → Format as Email

Prompt 1: "Summarize this meeting transcript in 200 words: {transcript}"
Prompt 2: "From this summary, extract the top 5 key decisions: {summary}"
Prompt 3: "For each decision, generate an action item with owner and deadline: {decisions}"
Prompt 4: "Format these action items as a professional follow-up email: {action_items}"
```

### Pas 2: Implementare secvențială
```python
def chain_execute(transcript):
    # Step 1: Summarize
    summary = llm(f"Summarize this meeting transcript in 200 words:\n{transcript}")

    # Step 2: Extract
    decisions = llm(f"Extract the top 5 key decisions from this summary:\n{summary}")

    # Step 3: Generate actions
    actions = llm(f"For each decision, create an action item (who, what, when):\n{decisions}")

    # Step 4: Format
    email = llm(f"Format as a professional follow-up email:\n{actions}")

    return email
```

### Pas 3: Chain cu branching (condiții)
```python
def conditional_chain(input_text):
    # Step 1: Classify
    category = llm(f"Classify this text as: technical, business, or personal.\n{input_text}")

    # Step 2: Branch based on category
    if "technical" in category.lower():
        result = llm(f"Provide a technical analysis:\n{input_text}")
    elif "business" in category.lower():
        result = llm(f"Provide a business impact analysis:\n{input_text}")
    else:
        result = llm(f"Summarize the key personal points:\n{input_text}")

    return result
```

### Pas 4: Chain cu LangChain
```python
from langchain.chains import SequentialChain, LLMChain

chain1 = LLMChain(llm=llm, prompt=summary_prompt, output_key="summary")
chain2 = LLMChain(llm=llm, prompt=extract_prompt, output_key="key_points")
chain3 = LLMChain(llm=llm, prompt=action_prompt, output_key="actions")

pipeline = SequentialChain(
    chains=[chain1, chain2, chain3],
    input_variables=["transcript"],
    output_variables=["summary", "key_points", "actions"]
)

result = pipeline({"transcript": meeting_text})
```

### Pas 5: Error handling în chains
```python
def robust_chain(input_text, max_retries=2):
    for step_fn in chain_steps:
        for attempt in range(max_retries):
            try:
                input_text = step_fn(input_text)
                break
            except Exception as e:
                if attempt == max_retries - 1:
                    return f"Chain failed at {step_fn.__name__}: {e}"
    return input_text
```

### Pas 6: Monitorizează și debugează
Loghează output-ul fiecărui pas pentru debugging:
```python
def traced_chain(input_text):
    trace = {"input": input_text}
    for i, step in enumerate(steps):
        output = step(input_text)
        trace[f"step_{i+1}"] = {"input": input_text, "output": output}
        input_text = output
    trace["final"] = input_text
    return input_text, trace
```

## Verificare

- [ ] Chain-ul e definit cu input/output clar per pas
- [ ] Fiecare pas testabil independent
- [ ] Error handling implementat
- [ ] Output-ul final e coerent
- [ ] Tracing/logging disponibil pentru debugging

## Instrumente

| Instrument | Rol |
|-----------|-----|
| LangChain SequentialChain | Orchestrare chains |
| Python | Custom pipeline |
| LangSmith | Tracing și debugging |

## Note

- **Când să folosești**: Pipelines de procesare; workflows multi-step; când fiecare pas necesită focus diferit.
- **Când NU**: Task-uri simple; când un singur prompt e suficient; overhead inutil.
- **Greșeli comune**: Prea mulți pași (fiecare pas pierde informație); lipsa error handling; ne-loghează intermediate outputs.
- **Relație**: ND-010 (Decomposition), PR-034 (DECOMP), PR-037 (Recursion-of-Thought).
