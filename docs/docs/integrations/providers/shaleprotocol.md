# Shale Protocol | Shale Protocol

[Shale Protocol](https://shaleprotocol.com)は、オープンなLLM向けに本番環境で使用可能な推論APIを提供しています。高いスケーラビリティを持つGPUクラウドインフラストラクチャ上でホストされているため、プラグアンドプレイのAPIとして利用できます。

> [Shale Protocol](https://shaleprotocol.com) provides production-ready inference APIs for open LLMs. It's a Plug & Play API as it's hosted on a highly scalable GPU cloud infrastructure.

私たちは、誰もがLLMを使用してgenAIアプリを構築し始める際の障壁を取り除くことを望んでいるため、無料プランではキーごとに1日最大1Kのリクエストをサポートしています。

> Our free tier supports up to 1K daily requests per key as we want to eliminate the barrier for anyone to start building genAI apps with LLMs.

Shale Protocolを使用することで、開発者や研究者は無償でアプリを作成し、オープンなLLMの機能を探求することができます。

> With Shale Protocol, developers/researchers can create apps and explore the capabilities of open LLMs at no cost.

このページでは、Shale-Serve APIをLangChainに組み込む方法について説明しています。

> This page covers how Shale-Serve API can be incorporated with LangChain.

2023年6月現在、APIはデフォルトでVicuna-13Bをサポートしています。将来のリリースで、Falcon-40Bなどの他のLLMもサポート予定です。

> As of June 2023, the API supports Vicuna-13B by default. We are going to support more LLMs such as Falcon-40B in future releases.

## How to | 方法

### 1. Find the link to our Discord on https://shaleprotocol.com. Generate an API key through the "Shale Bot" on our Discord. No credit card is required and no free trials. It's a forever free tier with 1K limit per day per API key. | https://shaleprotocol.com で私たちのDiscordへのリンクを見つけ、Discord上の「Shale Bot」を通じてAPIキーを生成してください。クレジットカードは必要なく、無料トライアルもありません。これは1日あたりAPIキーごとに1Kの制限がある永久無料のプランです。

### 2. Use https://shale.live/v1 as OpenAI API drop-in replacement | 2. OpenAI APIの代替としてhttps://shale.live/v1を使用してください

例えば

> For example

```python
from langchain.llms import OpenAI
from langchain.prompts import PromptTemplate
from langchain.chains import LLMChain

import os
os.environ['OPENAI_API_BASE'] = "https://shale.live/v1"
os.environ['OPENAI_API_KEY'] = "ENTER YOUR API KEY"

llm = OpenAI()

template = """Question: {question}

# Answer: Let's think step by step."""

prompt = PromptTemplate(template=template, input_variables=["question"])

llm_chain = LLMChain(prompt=prompt, llm=llm)

question = "What NFL team won the Super Bowl in the year Justin Beiber was born?"

llm_chain.run(question)

```
