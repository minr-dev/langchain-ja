# Streamlit | Streamlit

> **[Streamlit](https://streamlit.io/)は、データアプリを構築して共有するためのより速い方法です。**
> Streamlitは、数分でデータスクリプトを共有可能なウェブアプリに変換します。全てが純粋なPythonで実現されます。フロントエンドの経験は不要です。
> さらに多くの例は[streamlit.io/generative-ai](https://streamlit.io/generative-ai)でご覧いただけます。
>
> > **[Streamlit](https://streamlit.io/) is a faster way to build and share data apps.**
> > Streamlit turns data scripts into shareable web apps in minutes. All in pure Python. No front‑end experience required.
> > See more examples at [streamlit.io/generative-ai](https://streamlit.io/generative-ai).

[![GitHub Codespacesで開く](https://github.com/codespaces/badge.svg)](https://codespaces.new/langchain-ai/streamlit-agent?quickstart=1)

> [![Open in GitHub Codespaces](https://github.com/codespaces/badge.svg)](https://codespaces.new/langchain-ai/streamlit-agent?quickstart=1)

このガイドでは、インタラクティブなStreamlitアプリでエージェントの思考と行動を表示する方法を`StreamlitCallbackHandler`を使用して実演します。以下の実行中のアプリで、[MRKLエージェント](/docs/modules/agents/how_to/mrkl/)を使って試してみてください。

> In this guide we will demonstrate how to use `StreamlitCallbackHandler` to display the thoughts and actions of an agent in an
> interactive Streamlit app. Try it out with the running app below using the [MRKL agent](/docs/modules/agents/how_to/mrkl/):

<iframe loading="lazy" src="https://langchain-mrkl.streamlit.app/?embed=true&embed_options=light_theme"
    style={{ width: 100 + '%', border: 'none', marginBottom: 1 + 'rem', height: 600 }}
    allow="camera;clipboard-read;clipboard-write;"
></iframe>

## Installation and Setup | インストールとセットアップ

```bash
pip install langchain streamlit
```

`streamlit hello`を実行してサンプルアプリをロードし、インストールが成功したことを確認できます。Streamlitの[入門ドキュメント](https://docs.streamlit.io/library/get-started)で完全な手順を確認してください。

> You can run `streamlit hello` to load a sample app and validate your install succeeded. See full instructions in Streamlit's
> [Getting started documentation](https://docs.streamlit.io/library/get-started).

## Display thoughts and actions | 思考と行動を表示する

`StreamlitCallbackHandler`を作成するには、出力をレンダリングする親コンテナを提供するだけで十分です。

> To create a `StreamlitCallbackHandler`, you just need to provide a parent container to render the output.

```python
from langchain.callbacks import StreamlitCallbackHandler
import streamlit as st

st_callback = StreamlitCallbackHandler(st.container())
```

表示の振る舞いをカスタマイズするための追加のキーワード引数については、[APIリファレンス](https://api.python.langchain.com/en/latest/callbacks/langchain.callbacks.streamlit.streamlit_callback_handler.StreamlitCallbackHandler.html)で説明されています。

> Additional keyword arguments to customize the display behavior are described in the
> [API reference](https://api.python.langchain.com/en/latest/callbacks/langchain.callbacks.streamlit.streamlit_callback_handler.StreamlitCallbackHandler.html).

### Scenario 1: Using an Agent with Tools | シナリオ1: ツールを使ったエージェントの使用

現在主にサポートされているユースケースは、ツール（またはエージェントエグゼキュータ）を用いたエージェントのアクションの可視化です。Streamlitアプリ内でエージェントを作成し、`StreamlitCallbackHandler`を`agent.run()`に渡すことで、アプリ内でエージェントの思考とアクションをリアルタイムで可視化できます。

> The primary supported use case today is visualizing the actions of an Agent with Tools (or Agent Executor). You can create an
> agent in your Streamlit app and simply pass the `StreamlitCallbackHandler` to `agent.run()` in order to visualize the
> thoughts and actions live in your app.

```python
from langchain.llms import OpenAI
from langchain.agents import AgentType, initialize_agent, load_tools
from langchain.callbacks import StreamlitCallbackHandler
import streamlit as st

llm = OpenAI(temperature=0, streaming=True)
tools = load_tools(["ddg-search"])
agent = initialize_agent(
    tools, llm, agent=AgentType.ZERO_SHOT_REACT_DESCRIPTION, verbose=True
)

if prompt := st.chat_input():
    st.chat_message("user").write(prompt)
    with st.chat_message("assistant"):
        st_callback = StreamlitCallbackHandler(st.container())
        response = agent.run(prompt, callbacks=[st_callback])
        st.write(response)
```

注意：上記のアプリコードを正常に実行するためには、`OPENAI_API_KEY`を設定する必要があります。これを行う最も簡単な方法は、[Streamlitのsecrets.toml](https://docs.streamlit.io/library/advanced-features/secrets-management)を利用すること、または他のローカル環境管理ツールを使用することです。

> **Note:** You will need to set `OPENAI_API_KEY` for the above app code to run successfully.
> The easiest way to do this is via [Streamlit secrets.toml](https://docs.streamlit.io/library/advanced-features/secrets-management),
> or any other local ENV management tool.

### Additional scenarios | 追加のシナリオ

現在の`StreamlitCallbackHandler`は、LangChainエージェントエグゼキューターとの使用に特化しています。将来的には、さらなるエージェントタイプのサポートや、チェーンとの直接的な使用などが追加される予定です。

> Currently `StreamlitCallbackHandler` is geared towards use with a LangChain Agent Executor. Support for additional agent types,
> use directly with Chains, etc will be added in the future.

LangChainで[StreamlitChatMessageHistory](/docs/integrations/memory/streamlit_chat_message_history)を使用することにも興味があるかもしれません。

> You may also be interested in using
> [StreamlitChatMessageHistory](/docs/integrations/memory/streamlit_chat_message_history) for LangChain.
