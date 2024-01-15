# LLMonitor | LLMonitor

> [LLMonitor](https://llmonitor.com?utm_source=langchain\&utm_medium=py\&utm_campaign=docs)は、コストと使用状況の分析、ユーザー追跡、トレーシング、評価ツールを提供するオープンソースの監視プラットフォームです。
>
> > [LLMonitor](https://llmonitor.com?utm_source=langchain\&utm_medium=py\&utm_campaign=docs) is an open-source observability platform that provides cost and usage analytics, user tracking, tracing and evaluation tools.

<video controls width='100%' >
  <source src='https://llmonitor.com/videos/demo-annotated.mp4'/>
</video>

## Setup | セットアップ

[llmonitor.com](https://llmonitor.com?utm_source=langchain\&utm_medium=py\&utm_campaign=docs)にアカウントを作成し、新しいアプリの`tracking id`をコピーしてください。

> Create an account on [llmonitor.com](https://llmonitor.com?utm_source=langchain\&utm_medium=py\&utm_campaign=docs), then copy your new app's `tracking id`.

取得したら、以下のコマンドを実行して環境変数として設定してください：

> Once you have it, set it as an environment variable by running:

```bash
export LLMONITOR_APP_ID="..."
```

環境変数を設定したくない場合は、コールバックハンドラを初期化する際に直接キーを渡すことができます。

> If you'd prefer not to set an environment variable, you can pass the key directly when initializing the callback handler:

```python
from langchain.callbacks import LLMonitorCallbackHandler

handler = LLMonitorCallbackHandler(app_id="...")
```

## Usage with LLM/Chat models | LLM/Chatモデルとの使用方法

```python
from langchain.llms import OpenAI
from langchain.chat_models import ChatOpenAI
from langchain.callbacks import LLMonitorCallbackHandler

handler = LLMonitorCallbackHandler()

llm = OpenAI(
    callbacks=[handler],
)

chat = ChatOpenAI(callbacks=[handler])

llm("Tell me a joke")

```

## Usage with chains and agents | チェーンとエージェントの使用方法

`run` メソッドにコールバックハンドラを渡すことで、関連するチェーンとllmコールが正しく追跡されるようにしてください。

> Make sure to pass the callback handler to the `run` method so that all related chains and llm calls are correctly tracked.

ダッシュボードでエージェントを区別するためには、メタデータに`agent_name`を渡すことが推奨されます。

> It is also recommended to pass `agent_name` in the metadata to be able to distinguish between agents in the dashboard.

例:

> Example:

```python
from langchain.chat_models import ChatOpenAI
from langchain.schema import SystemMessage, HumanMessage
from langchain.agents import OpenAIFunctionsAgent, AgentExecutor, tool
from langchain.callbacks import LLMonitorCallbackHandler

llm = ChatOpenAI(temperature=0)

handler = LLMonitorCallbackHandler()

@tool
def get_word_length(word: str) -> int:
    """Returns the length of a word."""
    return len(word)

tools = [get_word_length]

prompt = OpenAIFunctionsAgent.create_prompt(
    system_message=SystemMessage(
        content="You are very powerful assistant, but bad at calculating lengths of words."
    )
)

agent = OpenAIFunctionsAgent(llm=llm, tools=tools, prompt=prompt, verbose=True)
agent_executor = AgentExecutor(
    agent=agent, tools=tools, verbose=True, metadata={"agent_name": "WordCount"}  # <- recommended, assign a custom name
)
agent_executor.run("how many letters in the word educa?", callbacks=[handler])
```

別の例：

> Another example:

```python
from langchain.agents import load_tools, initialize_agent, AgentType
from langchain.llms import OpenAI
from langchain.callbacks import LLMonitorCallbackHandler

handler = LLMonitorCallbackHandler()

llm = OpenAI(temperature=0)
tools = load_tools(["serpapi", "llm-math"], llm=llm)
agent = initialize_agent(tools, llm, agent=AgentType.ZERO_SHOT_REACT_DESCRIPTION, metadata={ "agent_name": "GirlfriendAgeFinder" })  # <- recommended, assign a custom name

agent.run(
    "Who is Leo DiCaprio's girlfriend? What is her current age raised to the 0.43 power?",
    callbacks=[handler],
)
```

## User Tracking | ユーザー追跡

ユーザートラッキングを利用することで、ユーザーを識別し、彼らのコスト、会話などを追跡することができます。

> User tracking allows you to identify your users, track their cost, conversations and more.

```python
from langchain.callbacks.llmonitor_callback import LLMonitorCallbackHandler, identify

with identify("user-123"):
    llm("Tell me a joke")

with identify("user-456", user_props={"email": "user456@test.com"}):
    agen.run("Who is Leo DiCaprio's girlfriend?")
```

## Support | サポート

統合に関する質問や問題がある場合は、[Discord](http://discord.com/invite/8PafSG58kK)または[email](mailto:vince@llmonitor.com)を通じてLLMonitorチームにお問い合わせいただけます。

> For any question or issue with integration you can reach out to the LLMonitor team on [Discord](http://discord.com/invite/8PafSG58kK) or via [email](mailto:vince@llmonitor.com).
