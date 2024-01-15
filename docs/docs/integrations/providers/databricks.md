# Databricks | Databricks

[Databricks](https://www.databricks.com/) Lakehouse Platformは、データ、アナリティクス、AIを一つのプラットフォームで統合しています。

> The [Databricks](https://www.databricks.com/) Lakehouse Platform unifies data, analytics, and AI on one platform.

Databricksは、さまざまな方法でLangChainエコシステムを取り入れています。

> Databricks embraces the LangChain ecosystem in various ways:

1. SQLDatabase Chain用のDatabricksコネクタ：SQLDatabase.from\_databricks()は、LangChainを通じてDatabricks上のデータを簡単にクエリする方法を提供します
   > Databricks connector for the SQLDatabase Chain: SQLDatabase.from\_databricks() provides an easy way to query your data on Databricks through LangChain
2. Databricks MLflowはLangChainと統合され、LangChainアプリケーションの追跡とサービングがより少ない手順で可能になりました
   > Databricks MLflow integrates with LangChain: Tracking and serving LangChain applications with fewer steps
3. DatabricksをLLMプロバイダーとして利用する：あなたのファインチューニングされたLLMをDatabricks上でサービングエンドポイントやクラスタードライバープロキシアプリケーションを通じてデプロイし、langchain.llms.Databricksでクエリします
   > Databricks as an LLM provider: Deploy your fine-tuned LLMs on Databricks via serving endpoints or cluster driver proxy apps, and query it as langchain.llms.Databricks
4. Databricks Dolly: Databricksは商用利用を許可するDollyをオープンソース化し、Hugging Face Hubを通じてアクセス可能です
   > Databricks Dolly: Databricks open-sourced Dolly which allows for commercial use, and can be accessed through the Hugging Face Hub

## Databricks connector for the SQLDatabase Chain | SQLDatabase Chain用のDatabricksコネクタ

LangChainのSQLDatabaseラッパーを使用して、[Databricks runtimes](https://docs.databricks.com/runtime/index.html)および[Databricks SQL](https://www.databricks.com/product/databricks-sql)に接続することができます。詳細は、ノートブック[Connect to Databricks](/docs/use_cases/qa_structured/integrations/databricks)を参照してください。

> You can connect to [Databricks runtimes](https://docs.databricks.com/runtime/index.html) and [Databricks SQL](https://www.databricks.com/product/databricks-sql) using the SQLDatabase wrapper of LangChain.
> See the notebook [Connect to Databricks](/docs/use_cases/qa_structured/integrations/databricks) for details.

## Databricks MLflow integrates with LangChain | Databricks MLflowはLangChainと統合しています

MLflowは、実験、再現性、デプロイメント、および中央モデルレジストリを含むMLライフサイクルを管理するためのオープンソースプラットフォームです。LangChainとのMLflowの統合についての詳細は、ノートブック[MLflow Callback Handler](/docs/integrations/providers/mlflow_tracking)をご覧ください。

> MLflow is an open-source platform to manage the ML lifecycle, including experimentation, reproducibility, deployment, and a central model registry. See the notebook [MLflow Callback Handler](/docs/integrations/providers/mlflow_tracking) for details about MLflow's integration with LangChain.

Databricksは、エンタープライズセキュリティ機能、高可用性、実験と実行管理、ノートブックのリビジョンキャプチャなどのDatabricksワークスペース機能と統合された、完全に管理されホストされたMLflowのバージョンを提供しています。Databricks上のMLflowは、機械学習モデルのトレーニング実行の追跡とセキュリティの確保、機械学習プロジェクトの実行に関して統合された体験を提供します。詳細は[MLflowガイド](https://docs.databricks.com/mlflow/index.html)をご覧ください。

> Databricks provides a fully managed and hosted version of MLflow integrated with enterprise security features, high availability, and other Databricks workspace features such as experiment and run management and notebook revision capture. MLflow on Databricks offers an integrated experience for tracking and securing machine learning model training runs and running machine learning projects. See [MLflow guide](https://docs.databricks.com/mlflow/index.html) for more details.

Databricks MLflowは、Databricks上でLangChainアプリケーションを開発することをより便利にします。MLflowトラッキングでは、トラッキングURIを設定する必要がありません。MLflow Model Servingでは、LangChainチェーンをMLflow langchainフレーバーで保存し、その後Databricks上で数クリックするだけでチェーンを登録して提供できます。認証情報はMLflow Model Servingによって安全に管理されます。

> Databricks MLflow makes it more convenient to develop LangChain applications on Databricks. For MLflow tracking, you don't need to set the tracking uri. For MLflow Model Serving, you can save LangChain Chains in the MLflow langchain flavor, and then register and serve the Chain with a few clicks on Databricks, with credentials securely managed by MLflow Model Serving.

## Databricks External Models | Databricks 外部モデル

[Databricks External Models](https://docs.databricks.com/generative-ai/external-models/index.html)は、OpenAIやAnthropicなどのさまざまな大規模言語モデル（LLM）プロバイダーの使用と管理を効率化するように設計されたサービスです。このサービスは、特定のLLM関連のリクエストを処理する統一されたエンドポイントを提供することにより、これらのサービスとのインタラクションを簡素化する高レベルのインターフェースを提供します。以下の例では、OpenAIのGPT-4モデルを利用するエンドポイントを作成し、そこからチャット応答を生成する方法を示しています：

> [Databricks External Models](https://docs.databricks.com/generative-ai/external-models/index.html) is a service that is designed to streamline the usage and management of various large language model (LLM) providers, such as OpenAI and Anthropic, within an organization. It offers a high-level interface that simplifies the interaction with these services by providing a unified endpoint to handle specific LLM related requests. The following example creates an endpoint that serves OpenAI's GPT-4 model and generates a chat response from it:

```python
from langchain.chat_models import ChatDatabricks
from langchain_core.messages import HumanMessage
from mlflow.deployments import get_deploy_client


client = get_deploy_client("databricks")
name = f"chat"
client.create_endpoint(
    name=name,
    config={
        "served_entities": [
            {
                "name": "test",
                "external_model": {
                    "name": "gpt-4",
                    "provider": "openai",
                    "task": "llm/v1/chat",
                    "openai_config": {
                        "openai_api_key": "{{secrets/<scope>/<key>}}",
                    },
                },
            }
        ],
    },
)
chat = ChatDatabricks(endpoint=name, temperature=0.1)
print(chat([HumanMessage(content="hello")]))
# -> content='Hello! How can I assist you today?'
```

## Databricks Foundation Model APIs | Databricks Foundation Model APIs

[Databricks Foundation Model APIs](https://docs.databricks.com/machine-learning/foundation-models/index.html)は、専用のサービングエンドポイントから最先端のオープンソースモデルにアクセスし、クエリを実行することができます。Foundation Model APIsを利用することで、開発者は自分でモデルのデプロイを管理することなく、高品質な生成AIモデルを活用したアプリケーションを迅速かつ容易に構築できます。以下の例では、`databricks-bge-large-en`エンドポイントを使用してテキストから埋め込みを生成しています：

> [Databricks Foundation Model APIs](https://docs.databricks.com/machine-learning/foundation-models/index.html) allow you to access and query state-of-the-art open source models from dedicated serving endpoints. With Foundation Model APIs, developers can quickly and easily build applications that leverage a high-quality generative AI model without maintaining their own model deployment. The following example uses the `databricks-bge-large-en` endpoint to generate embeddings from  text:

```python
from langchain.embeddings import DatabricksEmbeddings


embeddings = DatabricksEmbeddings(endpoint="databricks-bge-large-en")
print(embeddings.embed_query("hello")[:3])
# -> [0.051055908203125, 0.007221221923828125, 0.003879547119140625, ...]
```

## Databricks as an LLM provider | LLMプロバイダーとしてのDatabricks

ノートブック「[Wrap Databricks endpoints as LLMs](/docs/integrations/llms/databricks#wrapping-a-serving-endpoint-custom-model)」では、MLflowによって登録されたカスタムモデルをDatabricksエンドポイントとして提供する方法を説明しています。このノートブックは、本番環境と開発環境の両方に推奨されるサービングエンドポイントと、インタラクティブな開発に推奨されるクラスタドライバプロキシアプリという、2種類のエンドポイントをサポートしています。

> The notebook [Wrap Databricks endpoints as LLMs](/docs/integrations/llms/databricks#wrapping-a-serving-endpoint-custom-model) demonstrates how to serve a custom model that has been registered by MLflow as a Databricks endpoint.
> It supports two types of endpoints: the serving endpoint, which is recommended for both production and development, and the cluster driver proxy app, which is recommended for interactive development.

## Databricks Vector Search | Databricks ベクター検索

Databricks Vector Searchは、メタデータを含むデータのベクトル表現をベクトルデータベースに保存できるサーバーレス類似性検索エンジンです。Vector Searchを使用すると、Unity Catalogによって管理されるDeltaテーブルから自動更新されるベクトル検索インデックスを作成し、簡単なAPIを使って最も類似したベクトルを返すクエリを実行できます。LangChainでの使用方法については、ノートブック[Databricks Vector Search](/docs/integrations/vectorstores/databricks_vector_search)の指示に従ってください。

> Databricks Vector Search is a serverless similarity search engine that allows you to store a vector representation of your data, including metadata, in a vector database. With Vector Search, you can create auto-updating vector search indexes from Delta tables managed by Unity Catalog and query them with a simple API to return the most similar vectors. See the notebook [Databricks Vector Search](/docs/integrations/vectorstores/databricks_vector_search) for instructions to use it with LangChain.
