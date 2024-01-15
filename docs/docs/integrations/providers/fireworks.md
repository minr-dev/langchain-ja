# Fireworks | 花火

このページでは、Langchain内で[Fireworks](https://app.fireworks.ai/)モデルを使用する方法について説明します。

> This page covers how to use [Fireworks](https://app.fireworks.ai/) models within
> Langchain.

## Installation and setup | インストールとセットアップ

* Fireworksクライアントライブラリをインストールしてください。

  > Install the Fireworks client library.

  ```
  pip install fireworks-ai
  ```

* [app.fireworks.ai](https://app.fireworks.ai) でサインアップして、Fireworks APIキーを取得してください。
  > Get a Fireworks API key by signing up at [app.fireworks.ai](https://app.fireworks.ai).

* FIREWORKS\_API\_KEY環境変数を設定することで認証します。
  > Authenticate by setting the FIREWORKS\_API\_KEY environment variable.

## Authentication | 認証

Fireworks APIキーを使用して認証する方法は2つあります：

> There are two ways to authenticate using your Fireworks API key:

1. `FIREWORKS_API_KEY` 環境変数を設定する。

   > Setting the `FIREWORKS_API_KEY` environment variable.

   ```python
   os.environ["FIREWORKS_API_KEY"] = "<KEY>"
   ```

2. Fireworks LLMモジュールで`fireworks_api_key`フィールドを設定してください。

   > Setting `fireworks_api_key` field in the Fireworks LLM module.

   ```python
   llm = Fireworks(fireworks_api_key="<KEY>")
   ```

## Using the Fireworks LLM module | Fireworks LLMモジュールの使用

FireworksはLLMモジュールを通じてLangchainと統合されています。この例では、llama-v2-13b-chatモデルを扱います。

> Fireworks integrates with Langchain through the LLM module. In this example, we
> will work the llama-v2-13b-chat model.

```python
from langchain.llms.fireworks import Fireworks 

llm = Fireworks(
    fireworks_api_key="<KEY>",
    model="accounts/fireworks/models/llama-v2-13b-chat",
    max_tokens=256)
llm("Name 3 sports.")
```

より詳細な手順については、[こちら](/docs/integrations/llms/Fireworks)をご覧ください。

> For a more detailed walkthrough, see [here](/docs/integrations/llms/Fireworks).
