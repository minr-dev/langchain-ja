# Remembrall | リメンバーボール

このページでは、LangChain内で[Remembrall](https://remembrall.dev)エコシステムを使用する方法について説明します。

> This page covers how to use the [Remembrall](https://remembrall.dev) ecosystem within LangChain.

## What is Remembrall? | Remembrallとは何ですか？

Remembrallは、わずか数行のコードで、言語モデルに長期記憶、検索拡張生成、そして完全な可観測性を提供します。

> Remembrall gives your language model long-term memory, retrieval augmented generation, and complete observability with just a few lines of code.

![Remembrall Dashboard](/img/RemembrallDashboard.png)

これは、OpenAIの呼び出しの上に軽量なプロキシとして機能し、実行時にチャット呼び出しのコンテキストに収集された関連する事実を単に追加するものです。

> It works as a light-weight proxy on top of your OpenAI calls and simply augments the context of the chat calls at runtime with relevant facts that have been collected.

## Setup | セットアップ

始めるには、[RemembrallプラットフォームでGithubにサインイン](https://remembrall.dev/login)し、[設定ページからAPIキーをコピーしてください](https://remembrall.dev/dashboard/settings)。

> To get started, [sign in with Github on the Remembrall platform](https://remembrall.dev/login) and copy your [API key from the settings page](https://remembrall.dev/dashboard/settings).

以下に示すように変更された `openai_api_base` とRemembrall APIキーを使用して送信されたリクエストは、自動的にRemembrallダッシュボードで追跡されます。あなたのOpenAIキーを私たちのプラットフォームと共有する必要は**決して**ありませんし、この情報がRemembrallシステムによって保存されることも**決して**ありません。

> Any request that you send with the modified `openai_api_base` (see below) and Remembrall API key will automatically be tracked in the Remembrall dashboard. You **never** have to share your OpenAI key with our platform and this information is **never** stored by the Remembrall systems.

### Enable Long Term Memory | 長期記憶を有効にする

`openai_api_base`と`x-gp-api-key`を通じてRemembrall APIキーを設定するだけでなく、記憶を保持するためのUIDも指定する必要があります。これは通常、ユニークなユーザー識別子（例えばメールアドレス）になります。

> In addition to setting the `openai_api_base` and Remembrall API key via `x-gp-api-key`, you should specify a UID to maintain memory for. This will usually be a unique user identifier (like email).

```python
from langchain.chat_models import ChatOpenAI
chat_model = ChatOpenAI(openai_api_base="https://remembrall.dev/api/openai/v1",
                        model_kwargs={
                            "headers":{
                                "x-gp-api-key": "remembrall-api-key-here",
                                "x-gp-remember": "user@email.com",
                            }
                        })

chat_model.predict("My favorite color is blue.")
import time; time.sleep(5)  # wait for system to save fact via auto save
print(chat_model.predict("What is my favorite color?"))
```

### Enable Retrieval Augmented Generation | 検索強化生成を有効にする

まず、[Remembrall dashboard](https://remembrall.dev/dashboard/spells)でドキュメントコンテキストを作成してください。ドキュメントのテキストを貼り付けるか、PDFファイルとしてドキュメントをアップロードして処理してください。ドキュメントコンテキストIDを保存し、以下に示すように挿入してください。

> First, create a document context in the [Remembrall dashboard](https://remembrall.dev/dashboard/spells). Paste in the document texts or upload documents as PDFs to be processed. Save the Document Context ID and insert it as shown below.

```python
from langchain.chat_models import ChatOpenAI
chat_model = ChatOpenAI(openai_api_base="https://remembrall.dev/api/openai/v1",
                        model_kwargs={
                            "headers":{
                                "x-gp-api-key": "remembrall-api-key-here",
                                "x-gp-context": "document-context-id-goes-here",
                            }
                        })

print(chat_model.predict("This is a question that can be answered with my document."))
```
