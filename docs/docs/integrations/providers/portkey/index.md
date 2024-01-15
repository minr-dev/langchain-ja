# Portkey | ポートキー

> [Portkey](https://docs.portkey.ai/overview/introduction)は、Generative AIアプリケーションの展開と管理を効率化するために設計されたプラットフォームです。モデルの監視、管理、およびAIアプリケーションの性能向上のための包括的な機能を提供します。
>
> > [Portkey](https://docs.portkey.ai/overview/introduction) is a platform designed to streamline the deployment
> > and management of Generative AI applications.
> > It provides comprehensive features for monitoring, managing models,
> > and improving the performance of your AI applications.

## LLMOps for Langchain | LangchainのためのLLMOps

PortkeyはLangchainに本番環境での使用に耐えうる準備をもたらします。Portkeyを使用することで、あなたは

> Portkey brings production readiness to Langchain. With Portkey, you can

* \[x] すべてのリクエストに対する詳細な**メトリクスとログ**を表示します。
  > \[x] view detailed **metrics & logs** for all requests,
* \[x] レイテンシとコストを削減するために、**セマンティックキャッシュ**を有効にしてください。
  > \[x] enable **semantic cache** to reduce latency & costs,
* \[x] 失敗したリクエストに対して自動的な**リトライとフォールバック**を実装する、
  > \[x] implement automatic **retries & fallbacks** for failed requests,
* リクエストに**カスタムタグ**を追加して、追跡と分析をより効果的に行い、[さらに詳しい情報はこちら](https://docs.portkey.ai)。
  > \[x] add **custom tags** to requests for better tracking and analysis and [more](https://docs.portkey.ai).

### Using Portkey with Langchain | PortkeyをLangchainで使用する

Portkeyを使用するのは、どのPortkey機能を使いたいか選択し、`headers=Portkey.Config`を使ってそれらを有効にし、LLMの呼び出しで渡すだけで非常に簡単です。

> Using Portkey is as simple as just choosing which Portkey features you want, enabling them via `headers=Portkey.Config` and passing it in your LLM calls.

まず初めに、[こちらからサインアップ](https://app.portkey.ai/login)して、Portkey APIキーを取得してください。（左上のプロフィールアイコンをクリックし、その後「APIキーをコピー」をクリックしてください）

> To start, get your Portkey API key by [signing up here](https://app.portkey.ai/login). (Click the profile icon on the top left, then click on "Copy API Key")

OpenAIにおいて、ログ機能を備えたシンプルな統合は以下のようになります：

> For OpenAI, a simple integration with logging feature would look like this:

```python
from langchain.llms import OpenAI
from langchain.utilities import Portkey

# Add the Portkey API Key from your account
headers = Portkey.Config(
    api_key = "<PORTKEY_API_KEY>"
)

llm = OpenAI(temperature=0.9, headers=headers)
llm.predict("What would be a good company name for a company that makes colorful socks?")
```

あなたのログは、[Portkey dashboard](https://app.portkey.ai)に記録されます。

> Your logs will be captured on your [Portkey dashboard](https://app.portkey.ai).

一般的なPortkey X Langchainの使用例としては、**特定のチェーンやエージェントを追跡し**、そのリクエストから発生したすべてのLLMコールを表示することです。

> A common Portkey X Langchain use case is to **trace a chain or an agent** and view all the LLM calls originating from that request.

### **Tracing Chains & Agents** | **チェーンとエージェントの追跡**

```python
from langchain.agents import AgentType, initialize_agent, load_tools  
from langchain.llms import OpenAI
from langchain.utilities import Portkey

# Add the Portkey API Key from your account
headers = Portkey.Config(
    api_key = "<PORTKEY_API_KEY>",
    trace_id = "fef659"
)

llm = OpenAI(temperature=0, headers=headers)  
tools = load_tools(["serpapi", "llm-math"], llm=llm)  
agent = initialize_agent(tools, llm, agent=AgentType.ZERO_SHOT_REACT_DESCRIPTION, verbose=True)  
  
# Let's test it out!  
agent.run("What was the high temperature in SF yesterday in Fahrenheit? What is that number raised to the .023 power?")
```

Portkeyダッシュボードで、トレースIDと共にリクエストのログを確認することができます：

> **You can see the requests' logs along with the trace id on Portkey dashboard:**

<img src="/img/portkey-dashboard.gif" height="250"/>
<img src="/img/portkey-tracing.png" height="250"/>

## Advanced Features | 高度な機能

1. **ログ記録:** Portkeyを通じて送信することで、あなたのLLMリクエストを自動的にすべて記録します。各リクエストログには`timestamp`（タイムスタンプ）、`model name`（モデル名）、`total cost`（総コスト）、`request time`（リクエスト時間）、`request json`（リクエストJSON）、`response json`（レスポンスJSON）、さらにPortkeyの追加機能が含まれています。
   > **Logging:** Log all your LLM requests automatically by sending them through Portkey. Each request log contains `timestamp`, `model name`, `total cost`, `request time`, `request json`, `response json`, and additional Portkey features.
2. **トレーシング:** 各リクエストにトレースIDを添付して送信でき、Portkeyダッシュボードのログで可視化することができます。また、各リクエストに**固有のトレースID**を設定することもできます。さらに、トレースIDに[ユーザーフィードバックを追記する](https://docs.portkey.ai/key-features/feedback-api)ことも可能です。
   > **Tracing:** Trace id can be passed along with each request and is visibe on the logs on Portkey dashboard. You can also set a **distinct trace id** for each request. You can [append user feedback](https://docs.portkey.ai/key-features/feedback-api) to a trace id as well.
3. **キャッシング:** 以前にサービスした顧客のクエリに対して、OpenAIに再度送信する代わりにキャッシュから応答します。完全に一致する文字列または意味的に類似した文字列に対応します。キャッシュはコストを節約し、レイテンシーを20倍削減することができます。
   > **Caching:** Respond to previously served customers queries from cache instead of sending them again to OpenAI. Match exact strings OR semantically similar strings. Cache can save costs and reduce latencies by 20x.
4. **リトライ:** APIリクエストが成功しなかった場合、自動的に\*\*`最大5回`**まで再処理を行います。ネットワークの過負荷を防ぐために、リトライの試行間隔を広げる**`指数バックオフ`\*\*戦略を使用します。
   > **Retries:** Automatically reprocess any unsuccessful API requests **`upto 5`** times. Uses an **`exponential backoff`** strategy, which spaces out retry attempts to prevent network overload.
5. **タグ付け:** 事前に定義されたタグを使用して、各ユーザーのインタラクションを非常に詳細に追跡し監査します。
   > **Tagging:** Track and audit each user interaction in high detail with predefined tags.

| 機能 | 設定キー | 値（タイプ） | 必須/任意 |
| -- | -- | -- | -- |
| APIキー | `api_key` | APIキー（`string`） | ✅ 必須 |
| [リクエスト追跡](https://docs.portkey.ai/key-features/request-tracing) | `trace_id` | カスタム`string` | ❔ 任意 |
| [自動リトライ](https://docs.portkey.ai/key-features/automatic-retries) | `retry_count` | `integer`
\[1,2,3,4,5] | ❔ 任意 |
| [キャッシュ有効化](https://docs.portkey.ai/key-features/request-caching) | `cache` | `simple` または `semantic` | ❔ 任意 |
| キャッシュ強制リフレッシュ | `cache_force_refresh` | `True` | ❔ 任意 |
| キャッシュ有効期限設定 | `cache_age` | `integer`（秒単位） | ❔ 任意 |
| [ユーザー追加](https://docs.portkey.ai/key-features/custom-metadata) | `user` | `string` | ❔ 任意 |
| [組織追加](https://docs.portkey.ai/key-features/custom-metadata) | `organisation` | `string` | ❔ 任意 |
| [環境追加](https://docs.portkey.ai/key-features/custom-metadata) | `environment` | `string` | ❔ 任意 |
| [プロンプト追加（バージョン/ID/文字列）](https://docs.portkey.ai/key-features/custom-metadata) | `prompt` | `string` | ❔ 任意 |

> | Feature | Config Key | Value (Type) | Required/Optional |
> | -- | -- | -- | -- |
> | API Key | `api_key` | API Key (`string`) | ✅ Required |
> | [Tracing Requests](https://docs.portkey.ai/key-features/request-tracing) | `trace_id` | Custom `string` | ❔ Optional |
> | [Automatic Retries](https://docs.portkey.ai/key-features/automatic-retries) | `retry_count` | `integer` \[1,2,3,4,5] | ❔ Optional |
> | [Enabling Cache](https://docs.portkey.ai/key-features/request-caching) | `cache` | `simple` OR `semantic` | ❔ Optional |
> | Cache Force Refresh | `cache_force_refresh` | `True` | ❔ Optional |
> | Set Cache Expiry | `cache_age` | `integer` (in seconds) | ❔ Optional |
> | [Add User](https://docs.portkey.ai/key-features/custom-metadata) | `user` | `string` | ❔ Optional |
> | [Add Organisation](https://docs.portkey.ai/key-features/custom-metadata) | `organisation` | `string` | ❔ Optional |
> | [Add Environment](https://docs.portkey.ai/key-features/custom-metadata) | `environment` | `string` | ❔ Optional |
> | [Add Prompt (version/id/string)](https://docs.portkey.ai/key-features/custom-metadata) | `prompt` | `string` | ❔ Optional |

## **Enabling all Portkey Features:** | **すべてのPortkey機能を有効にする:**

```py
headers = Portkey.Config(
    
    # Mandatory
    api_key="<PORTKEY_API_KEY>",  
	
	# Cache Options
    cache="semantic",                 
    cache_force_refresh="True",             
    cache_age=1729,  

    # Advanced
    retry_count=5,                                           
    trace_id="langchain_agent",                          

    # Metadata
    environment="production",        
    user="john",                      
    organisation="acme",             
    prompt="Frost"
    
)
```

各機能の詳細情報とその使用方法については、[Portkeyのドキュメントをご参照ください](https://docs.portkey.ai)。ご質問がある場合やさらなる支援が必要な場合は、[Twitterでお問い合わせください](https://twitter.com/portkeyai)。

> For detailed information on each feature and how to use it, [please refer to the Portkey docs](https://docs.portkey.ai). If you have any questions or need further assistance, [reach out to us on Twitter.](https://twitter.com/portkeyai).
