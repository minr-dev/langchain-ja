# Baseten | Baseten

[Baseten](https://baseten.co)は、MLモデルを高パフォーマンスかつスケーラブルに、コスト効率よく展開して提供するために必要なすべてのインフラストラクチャを提供します。

> [Baseten](https://baseten.co) provides all the infrastructure you need to deploy and serve ML models performantly, scalably, and cost-efficiently.

モデル推論プラットフォームとして、BasetenはLangChainエコシステム内の`Provider`です。現在、Basetenの統合は`Component`としてLLMsを一つだけ実装していますが、将来的にはさらに多くのコンポーネントが実装される予定です！

> As a model inference platform, Baseten is a `Provider` in the LangChain ecosystem. The Baseten integration currently implements a single `Component`, LLMs, but more are planned!

Basetenでは、Llama 2やMistralのようなオープンソースモデルだけでなく、専用のGPUでプロプライエタリモデルやファインチューニングされたモデルを実行することができます。OpenAIのようなプロバイダーに慣れている場合、Basetenを使用する際にはいくつかの違いがあります：

> Baseten lets you run both open source models like Llama 2 or Mistral and run proprietary or fine-tuned models on dedicated GPUs. If you're used to a provider like OpenAI, using Baseten has a few differences:

* トークンごとに支払うのではなく、使用したGPUの分数ごとに支払います。
  > Rather than paying per token, you pay per minute of GPU used.
* Basetenのすべてのモデルは、最大限のカスタマイズ性を実現するために、私たちのオープンソースモデルパッケージングフレームワークである[Truss](https://truss.baseten.co/welcome)を使用しています。
  > Every model on Baseten uses [Truss](https://truss.baseten.co/welcome), our open-source model packaging framework, for maximum customizability.
* 私たちはいくつかの[OpenAI ChatCompletions互換モデル](https://docs.baseten.co/api-reference/openai)を提供していますが、Trussを使用してあなた自身のI/O仕様を定義することもできます。
  > While we have some [OpenAI ChatCompletions-compatible models](https://docs.baseten.co/api-reference/openai), you can define your own I/O spec with Truss.

Basetenについてさらに詳しく知りたい場合は、[私たちのドキュメント](https://docs.baseten.co/)をご覧ください。また、LangChainに特化した情報が必要なら、引き続きお読みください。

> You can learn more about Baseten in [our docs](https://docs.baseten.co/) or read on for LangChain-specific info.

## Setup: LangChain + Baseten | セットアップ: LangChain + Baseten

LangChainでBasetenモデルを使用するためには、2つのものが必要です：

> You'll need two things to use Baseten models with LangChain:

* Basetenアカウント
  > A [Baseten account](https://baseten.co)
* APIキー（[API key](https://docs.baseten.co/observability/api-keys)）
  > An [API key](https://docs.baseten.co/observability/api-keys)

`BASETEN_API_KEY` という名前の環境変数として、あなたの API キーをエクスポートしてください。

> Export your API key to your as an environment variable called `BASETEN_API_KEY`.

```sh
export BASETEN_API_KEY="paste_your_api_key_here"
```

## Component guide: LLMs | コンポーネントガイド：LLMs

Basetenは、Basetenワークスペースにデプロイされたモデルに対して標準化された相互運用可能なインターフェースを提供する[LLMコンポーネント](https://python.langchain.com/docs/integrations/llms/baseten)を通じてLangChainと統合しています。

> Baseten integrates with LangChain through the [LLM component](https://python.langchain.com/docs/integrations/llms/baseten), which provides a standardized and interoperable interface for models that are deployed on your Baseten workspace.

MistralやLlama 2のような基盤モデルは、[Basetenモデルライブラリ](https://app.baseten.co/explore/)からワンクリックでデプロイできます。また、ご自身のモデルがある場合は、[Trussを使ってデプロイ](https://truss.baseten.co/welcome)することも可能です。

> You can deploy foundation models like Mistral and Llama 2 with one click from the [Baseten model library](https://app.baseten.co/explore/) or if you have your own model, [deploy it with Truss](https://truss.baseten.co/welcome).

この例では、Mistral 7Bを扱います。[こちらからMistral 7Bをデプロイ](https://app.baseten.co/explore/mistral_7b_instruct)して、モデルダッシュボードに表示されるデプロイ済みモデルのIDを参照しながら進めてください。

> In this example, we'll work with Mistral 7B. [Deploy Mistral 7B here](https://app.baseten.co/explore/mistral_7b_instruct) and follow along with the deployed model's ID, found in the model dashboard.

このモジュールを使用するには、以下の条件を満たす必要があります：

> To use this module, you must:

* Baseten APIキーを環境変数BASETEN\_API\_KEYとしてエクスポートしてください
  > Export your Baseten API key as the environment variable BASETEN\_API\_KEY
* BasetenダッシュボードからあなたのモデルのモデルIDを取得してください
  > Get the model ID for your model from your Baseten dashboard
* モデルライブラリのモデルすべてに対して、「production」をモデルデプロイメントとして識別する
  > Identify the model deployment ("production" for all model library models)

[モデルIDとデプロイメントについて](https://docs.baseten.co/deploy/lifecycle)もっと詳しく学びましょう。

> [Learn more](https://docs.baseten.co/deploy/lifecycle) about model IDs and deployments.

本番環境へのデプロイメント（モデルライブラリのモデルにとっての標準）

> Production deployment (standard for model library models)

```python
from langchain_community.llms import Baseten

mistral = Baseten(model="MODEL_ID", deployment="production")
mistral("What is the Mistral wind?")
```

開発環境へのデプロイ

> Development deployment

```python
from langchain_community.llms import Baseten

mistral = Baseten(model="MODEL_ID", deployment="development")
mistral("What is the Mistral wind?")
```

その他の公開済みデプロイメント

> Other published deployment

```python
from langchain_community.llms import Baseten

mistral = Baseten(model="MODEL_ID", deployment="DEPLOYMENT_ID")
mistral("What is the Mistral wind?")
```

ストリーミングLLM出力、チャット補完、埋め込みモデルなどは、Basetenプラットフォームでサポートされており、LangChain統合にも間もなく対応予定です。BasetenとLangChainを使用する際のご質問がある場合は、<support@baseten.co>までお問い合わせください。

> Streaming LLM output, chat completions, embeddings models, and more are all supported on the Baseten platform and coming soon to our LangChain integration. Contact us at <support@baseten.co> with any questions about using Baseten with LangChain.
