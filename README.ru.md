# Llm Api Examples

[English](README.md) | [Français](README.fr.md) | [Español](README.es.md) | [中文](README.zh-CN.md) | [日本語](README.ja.md) | **Русский**

Готовые примеры для API LLM

##   OpenAI
```python
from openai import OpenAI
client = OpenAI()
r = client.chat.completions.create(
    model="gpt-4o-mini",
    messages=[{"role":"user","content":"Hello"}])
print(r.choices[0].message.content)
```

## ) Антропик (Клод)
```python
import anthropic
c = anthropic.Anthropic()
m = c.messages.create(model="claude-sonnet-4-20250514", max_tokens=1024,
                      messages=[{"role":"user","content":"Hello"}])
print(m.content[0].text)
```

## Мистраль
```python
from mistralai import Mistral
m = Mistral(api_key="...")
r = m.chat.complete(model="mistral-small-latest",
                    messages=[{"role":"user","content":"Hello"}])
print(r.choices[0].message.content)
```

## ) Местный (Ollama — OpenAI-совместимый)
```python
from openai import OpenAI
client = OpenAI(base_url="http://localhost:11434/v1", api_key="ollama")
r = client.chat.completions.create(model="qwen3", messages=[{"role":"user","content":"Hello"}])
print(r.choices[0].message.content)
```

###Поток
```python
for chunk in client.chat.completions.create(model="gpt-4o-mini",
        messages=[{"role":"user","content":"count to 5"}], stream=True):
    if chunk.choices[0].delta.content: print(chunk.choices[0].delta.content, end="")
```

Функция вызова (OpenAI)
```python
tools=[{"type":"function","function":{"name":"get_weather","parameters":{"type":"object","properties":{"city":{"type":"string"}}}}}]
r = client.chat.completions.create(model="gpt-4o-mini", messages=[...], tools=tools)
```

Один клиент, все поставщики: [LiteLLM] (https://github.com/BerriAI/litellm)
```python
import litellm
r = litellm.completion(model="ollama/qwen3", messages=[{"role":"user","content":"hi"}])
```

##Сравнение поставщиков

| Провайдер | SDK | Лучшая модель | Заметки |
|---|---|---|---|
| OpenAI | `openai` | GPT-4o/o-серии | Справочная реализация |
| Антропный | `anthropic` | Клод Соннет/Опус Лучшая долгосрочная аргументация
| Google | `google-genai` | Gemini Pro | Длинный контекст
| Mistral | `mistralai` | Mistral Large | Вариант ЕС
| OpenRouter | `openai`-совместимый | Все они | Один ключ → каждая модель |

##  patterns Стандартные шаблоны

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

###Быстрый кэш

И OpenAI, и Anthropic кэшируют длинные префиксы — ставят системную подсказку + документы на первое место, переменную часть на последнее. 50-90% снижение затрат на RAG.

---
## Участие

Pull request'ы приветствуются — открой PR, он будет проверен и принят. Ведёт [aixisstudio](https://github.com/aixisstudio), автор [Snitch](https://github.com/aixisstudio/Snitch) — open-source application firewall.

## Лицензия: MIT
