# Llm Api Examples

[English](README.md) | [Français](README.fr.md) | **Español** | [中文](README.zh-CN.md) | [日本語](README.ja.md) | [Русский](README.ru.md)

Ejemplos listos para APIs de LLM

## 🔌 OpenAI
```python
from openai import OpenAI
client = OpenAI()
r = client.chat.completions.create(
    model="gpt-4o-mini",
    messages=[{"role":"user","content":"Hello"}])
print(r.choices[0].message.content)
```

## 🧠 Antropopic (Claude)
```python
import anthropic
c = anthropic.Anthropic()
m = c.messages.create(model="claude-sonnet-4-20250514", max_tokens=1024,
                      messages=[{"role":"user","content":"Hello"}])
print(m.content[0].text)
```

## 🇫 Desnudez Mistral
```python
from mistralai import Mistral
m = Mistral(api_key="...")
r = m.chat.complete(model="mistral-small-latest",
                    messages=[{"role":"user","content":"Hello"}])
print(r.choices[0].message.content)
```

##  tuya Local (Ollama — OpenAI-compatible)
```python
from openai import OpenAI
client = OpenAI(base_url="http://localhost:11434/v1", api_key="ollama")
r = client.chat.completions.create(model="qwen3", messages=[{"role":"user","content":"Hello"}])
print(r.choices[0].message.content)
```

## 🌊 Streaming
```python
for chunk in client.chat.completions.create(model="gpt-4o-mini",
        messages=[{"role":"user","content":"count to 5"}], stream=True):
    if chunk.choices[0].delta.content: print(chunk.choices[0].delta.content, end="")
```

## 🔧 Función llamando (OpenAI)
```python
tools=[{"type":"function","function":{"name":"get_weather","parameters":{"type":"object","properties":{"city":{"type":"string"}}}}}]
r = client.chat.completions.create(model="gpt-4o-mini", messages=[...], tools=tools)
```

## ♥ Un cliente, todos los proveedores: [LiteLLM](https://github.com/BerriAI/litellm)
```python
import litellm
r = litellm.completion(model="ollama/qwen3", messages=[{"role":"user","content":"hi"}])
```

## 🔄 Comparación de proveedores

Silencioso Proveedor Silencio SDK Silencio Mejor modelo
|---|---|---|---|
Silencio OpenAI Silencio `openai` Silencio GPT-4o/o-series 
Silencio Antrópico TENIDO `anthropic` ANTE Claude Sonnet/Opus El mejor razonamiento de larga duración
Silencio Google Silencio `google-genai` Silencio Gemini Pro Silencio Contexto largo, flash barato
Silencio Mistral Silencio `mistralai` Silencio Mistral Grande Silencioso
Silencio OpenRouter ← `openai`-compatible Silencio Todos ellos ← Una clave → cada modelo ←

## 🧰 Standard patterns

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

## 📏 Prompt caching

Tanto OpenAI como los prefijos largos de caché antropópico — poner el sistema rápido + docs primero, parte variable último. 50-90% de coste reducido en RAG.

---
## Contribuir

Los pull requests son bienvenidos: abre una PR y será revisada y fusionada. Mantenido por [aixisstudio](https://github.com/aixisstudio), autor de [Snitch](https://github.com/aixisstudio/Snitch), firewall de aplicaciones open source.

## Licencia: MIT
