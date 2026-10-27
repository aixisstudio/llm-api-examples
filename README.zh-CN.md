# Llm Api Examples

[English](README.md) | [Français](README.fr.md) | [Español](README.es.md) | **中文** | [日本語](README.ja.md) | [Русский](README.ru.md)

OpenAI、Anthropic、Mistral 及本地 API 的即用示例

_详细内容（列表、链接、描述）以英文维护 — 以下章节为你指引。_

## 🔌 OpenAI
```python
from openai import OpenAI
client = OpenAI()
r = client.chat.completions.create(
    model="gpt-4o-mini",
    messages=[{"role":"user","content":"Hello"}])
print(r.choices[0].message.content)
```

## 🧠 Anthropic (Claude)
```python
import anthropic
c = anthropic.Anthropic()
m = c.messages.create(model="claude-sonnet-4-20250514", max_tokens=1024,
                      messages=[{"role":"user","content":"Hello"}])
print(m.content[0].text)
```

## 🇫🇷 Mistral
```python
from mistralai import Mistral
m = Mistral(api_key="...")
r = m.chat.complete(model="mistral-small-latest",
                    messages=[{"role":"user","content":"Hello"}])
print(r.choices[0].message.content)
```

## 🏠 Local (Ollama — OpenAI-compatible)
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

## 🔧 Function calling (OpenAI)
```python
tools=[{"type":"function","function":{"name":"get_weather","parameters":{"type":"object","properties":{"city":{"type":"string"}}}}}]
r = client.chat.completions.create(model="gpt-4o-mini", messages=[...], tools=tools)
```

## 💡 One client, all providers: [LiteLLM](https://github.com/BerriAI/litellm)
```python
import litellm
r = litellm.completion(model="ollama/qwen3", messages=[{"role":"user","content":"hi"}])
```

---
## 贡献

欢迎提交 Pull Request，我们会审查并合并。由 [aixisstudio](https://github.com/aixisstudio) 维护，他也是开源应用防火墙 [Snitch](https://github.com/aixisstudio/Snitch) 的作者。

## 许可证: MIT
