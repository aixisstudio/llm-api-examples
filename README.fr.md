# Llm Api Examples

[English](README.md) | **Français** | [Español](README.es.md) | [中文](README.zh-CN.md) | [日本語](README.ja.md) | [Русский](README.ru.md)

Exemples copiables pour les APIs OpenAI, Anthropic, Mistral & locales

Ouverture de l'AI
```python
from openai import OpenAI
client = OpenAI()
r = client.chat.completions.create(
    model="gpt-4o-mini",
    messages=[{"role":"user","content":"Hello"}])
print(r.choices[0].message.content)
```

C'est une anthropique (Claude)
```python
import anthropic
c = anthropic.Anthropic()
m = c.messages.create(model="claude-sonnet-4-20250514", max_tokens=1024,
                      messages=[{"role":"user","content":"Hello"}])
print(m.content[0].text)
```

La Mistral
```python
from mistralai import Mistral
m = Mistral(api_key="...")
r = m.chat.complete(model="mistral-small-latest",
                    messages=[{"role":"user","content":"Hello"}])
print(r.choices[0].message.content)
```

Local (Ollama – compatible OpenAI)
```python
from openai import OpenAI
client = OpenAI(base_url="http://localhost:11434/v1", api_key="ollama")
r = client.chat.completions.create(model="qwen3", messages=[{"role":"user","content":"Hello"}])
print(r.choices[0].message.content)
```

Le flux
```python
for chunk in client.chat.completions.create(model="gpt-4o-mini",
        messages=[{"role":"user","content":"count to 5"}], stream=True):
    if chunk.choices[0].delta.content: print(chunk.choices[0].delta.content, end="")
```

Fonction appelant (OpenAI)
```python
tools=[{"type":"function","function":{"name":"get_weather","parameters":{"type":"object","properties":{"city":{"type":"string"}}}}}]
r = client.chat.completions.create(model="gpt-4o-mini", messages=[...], tools=tools)
```

Un seul client, tous les fournisseurs : [LiteLLM](https://github.com/BerriAI/litellm)
```python
import litellm
r = litellm.completion(model="ollama/qwen3", messages=[{"role":"user","content":"hi"}])
```

Comparaison des fournisseurs

Le meilleur modèle Notes
|---|---|---|---|
Ouverture de l'API `openai`
Claude Sonnet/Opus Meilleur raisonnement long-forme
Google (en anglais seulement) `google-genai` (en anglais seulement)
Mistral (en anglais seulement)
OpenRouter (en anglais seulement) `openai`-compatible (en anglais seulement)

Modèles standard

```python
# Streaming (all SDKs similar)
stream = client.chat.completions.create(model="gpt-4o", messages=msgs, stream=True)
for chunk in stream:
    print(chunk.choices[0].delta.content or "", end="")

# Retry with backoff — 429s are normal
from tenacity import retry, wait_exponential
@retry(wait=wait_exponential(min=1, max=60))
def call(): return client.chat.completions.create(...)

# Cost tracking
usage = resp.usage  # prompt_tokens, completion_tokens — log these
```

Cache rapide

OpenAI et Anthropic cache préfixes longs — mettre le système invite + docs en premier, la partie variable en dernier. Réduction des coûts de RAG de 50 à 90 %.

---
## Contribuer

Les pull requests sont les bienvenues : ouvre une PR, elle sera relue et fusionnée. Maintenu par [aixisstudio](https://github.com/aixisstudio), auteur de [Snitch](https://github.com/aixisstudio/Snitch), pare-feu applicatif open source.

## Licence: MIT
