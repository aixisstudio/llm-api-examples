# Llm Api Examples

[English](README.md) | [Français](README.fr.md) | [Español](README.es.md) | [中文](README.zh-CN.md) | **日本語** | [Русский](README.ru.md)

主要 LLM API のコピペ用サンプル集

##  Open オープンAI
```python
from openai import OpenAI
client = OpenAI()
r = client.chat.completions.create(
    model="gpt-4o-mini",
    messages=[{"role":"user","content":"Hello"}])
print(r.choices[0].message.content)
```

## 🧠 Anthropic (クロード)
```python
import anthropic
c = anthropic.Anthropic()
m = c.messages.create(model="claude-sonnet-4-20250514", max_tokens=1024,
                      messages=[{"role":"user","content":"Hello"}])
print(m.content[0].text)
```

## ユ・ミストラル
```python
from mistralai import Mistral
m = Mistral(api_key="...")
r = m.chat.complete(model="mistral-small-latest",
                    messages=[{"role":"user","content":"Hello"}])
print(r.choices[0].message.content)
```

## ローカル (Ollama — OpenAI 対応)
```python
from openai import OpenAI
client = OpenAI(base_url="http://localhost:11434/v1", api_key="ollama")
r = client.chat.completions.create(model="qwen3", messages=[{"role":"user","content":"Hello"}])
print(r.choices[0].message.content)
```

## 武道
```python
for chunk in client.chat.completions.create(model="gpt-4o-mini",
        messages=[{"role":"user","content":"count to 5"}], stream=True):
    if chunk.choices[0].delta.content: print(chunk.choices[0].delta.content, end="")
```

## ❀ 関数呼び出し(OpenAI)
```python
tools=[{"type":"function","function":{"name":"get_weather","parameters":{"type":"object","properties":{"city":{"type":"string"}}}}}]
r = client.chat.completions.create(model="gpt-4o-mini", messages=[...], tools=tools)
```

## 1つのクライアント、すべてのプロバイダ: [LiteLLM](https://github.com/BerriAI/litellm)
```python
import litellm
r = litellm.completion(model="ollama/qwen3", messages=[{"role":"user","content":"hi"}])
```

## クライアントの比較

| 提供者 | SDK | ベストモデル | ノート |
|---|---|---|---|
| OpenAI | `openai` | GPT-4o/oシリーズ | リファレンス実装 |
| 人類性 | `anthropic` | クロード・ソンネット | オーパス | 最長の理由 |
| Google | `google-genai` | ジェミニプロ | ロングコンテクスト・安いフラッシュ |
| ミストラル | `mistralai` | ミストラル大 | EUオプション |
| OpenRouter | `openai`対応 | 全て | ワンキー → 各モデル |

##標準パターン

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

## 通常のキャッシング

OpenAI と Anthropic キャッシュの長いプレフィックスの両方 — システムプロンプト + docs を最初に置き、変数部分は最後に続きます。 RAGで50-90%のコストカット。

---
## 貢献

プルリクエストを歓迎します。レビューしてマージします。[aixisstudio](https://github.com/aixisstudio) による管理 — オープンソースのアプリケーションファイアウォール [Snitch](https://github.com/aixisstudio/Snitch) の作者です。

## ライセンス: MIT
