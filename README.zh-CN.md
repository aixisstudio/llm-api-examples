# Llm Api Examples

[English](README.md) | [Français](README.fr.md) | [Español](README.es.md) | **中文** | [日本語](README.ja.md) | [Русский](README.ru.md)

OpenAI、Anthropic、Mistral 及本地 API 的即用示例

□ □ □ 开放AI
```python
from openai import OpenAI
client = OpenAI()
r = client.chat.completions.create(
    model="gpt-4o-mini",
    messages=[{"role":"user","content":"Hello"}])
print(r.choices[0].message.content)
```

□□□□□□□□□□□□□□□□□
```python
import anthropic
c = anthropic.Anthropic()
m = c.messages.create(model="claude-sonnet-4-20250514", max_tokens=1024,
                      messages=[{"role":"user","content":"Hello"}])
print(m.content[0].text)
```

□ □ □ 迷雾
```python
from mistralai import Mistral
m = Mistral(api_key="...")
r = m.chat.complete(model="mistral-small-latest",
                    messages=[{"role":"user","content":"Hello"}])
print(r.choices[0].message.content)
```

^ QQ 本地( Ollama – OpenAI 兼容)
```python
from openai import OpenAI
client = OpenAI(base_url="http://localhost:11434/v1", api_key="ollama")
r = client.chat.completions.create(model="qwen3", messages=[{"role":"user","content":"Hello"}])
print(r.choices[0].message.content)
```

□□□□□□□□□□□□□□□
```python
for chunk in client.chat.completions.create(model="gpt-4o-mini",
        messages=[{"role":"user","content":"count to 5"}], stream=True):
    if chunk.choices[0].delta.content: print(chunk.choices[0].delta.content, end="")
```

□ QQ 函数调用( OpenAI)
```python
tools=[{"type":"function","function":{"name":"get_weather","parameters":{"type":"object","properties":{"city":{"type":"string"}}}}}]
r = client.chat.completions.create(model="gpt-4o-mini", messages=[...], tools=tools)
```

□ □ □ 一个客户端, 所有提供者 : [LiteLLM] (https://github.com/BerriAI/litellm)
```python
import litellm
r = litellm.completion(model="ollama/qwen3", messages=[{"role":"user","content":"hi"}])
```

□ 提供方比较

SDK 最佳型号 注解
|---|---|---|---|
OpenAI `openai` GPT-4o/O-系列 参考执行 |
`anthropic` 克劳德·索内特/奥普斯 最佳长式推理
Google Google `google-genai` 双子座 双子座 长的上下文 便宜的闪存 QQ
米斯特尔 `mistralai` 欧盟的米斯特尔大选项
`openai` 兼容 全部 键 键 每个型号

□ □ □ 标准模式

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

□□□□□□□□□□

OpenAI和Anthropic缓存的长前缀——将系统提示+docs放在首位,变量部分放在最后. RAG削减50-90%成本.

---
## 贡献

欢迎提交 Pull Request，我们会审查并合并。由 [aixisstudio](https://github.com/aixisstudio) 维护，他也是开源应用防火墙 [Snitch](https://github.com/aixisstudio/Snitch) 的作者。

## 许可证: MIT
