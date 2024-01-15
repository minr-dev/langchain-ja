# Predibase | Predibase

Predibase上のモデルを使ってLangChainの使用方法を学びましょう。

> Learn how to use LangChain with models on Predibase.

## Setup | セットアップ

* [Predibase](https://predibase.com/)のアカウントを作成し、[APIキー](https://docs.predibase.com/sdk-guide/intro)を取得してください。
  > Create a [Predibase](https://predibase.com/) account and [API key](https://docs.predibase.com/sdk-guide/intro).
* `pip install predibase`でPredibase Pythonクライアントをインストールしてください
  > Install the Predibase Python client with `pip install predibase`
* APIキーを使用して認証してください
  > Use your API key to authenticate

### LLM | LLM

Predibaseは、LLMモジュールを実装することによりLangChainと統合されています。以下に簡単な例を示しますが、LLM > Integrations > Predibaseの下には完全なノートブックも用意されています。

> Predibase integrates with LangChain by implementing LLM module. You can see a short example below or a full notebook under LLM > Integrations > Predibase.

```python
import os
os.environ["PREDIBASE_API_TOKEN"] = "{PREDIBASE_API_TOKEN}"

from langchain.llms import Predibase

model =  Predibase(model = 'vicuna-13b', predibase_api_key=os.environ.get('PREDIBASE_API_TOKEN'))

response = model("Can you recommend me a nice dry wine?")
print(response)
```
