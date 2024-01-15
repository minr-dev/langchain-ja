# Marqo | Marqo

このページでは、LangChain内でMarqoエコシステムを使用する方法について説明しています。

> This page covers how to use the Marqo ecosystem within LangChain.

### **What is Marqo?** | **Marqoとは何ですか？**

Marqoは、メモリ内HNSWインデックスに格納された埋め込みを使用して、最先端の検索速度を実現するテンソル検索エンジンです。Marqoは、水平インデックスシャーディングにより億単位のドキュメントインデックスにスケールアップ可能で、非同期かつノンブロッキングなデータアップロードと検索を可能にします。Marqoは、PyTorch、Huggingface、OpenAIなどの最新の機械学習モデルを使用しています。事前に設定されたモデルから始めることも、自分のモデルを持ち込むこともできます。内蔵されたONNXサポートと変換により、CPUとGPUの両方でより高速な推論と高いスループットを実現します。

> Marqo is a tensor search engine that uses embeddings stored in in-memory HNSW indexes to achieve cutting edge search speeds. Marqo can scale to hundred-million document indexes with horizontal index sharding and allows for async and non-blocking data upload and search. Marqo uses the latest machine learning models from PyTorch, Huggingface, OpenAI and more. You can start with a pre-configured model or bring your own. The built in ONNX support and conversion allows for faster inference and higher throughput on both CPU and GPU.

Marqoには独自の推論機能が含まれているため、テキストと画像が混在したドキュメントを扱うことができます。また、他のシステムからのデータをMarqoインデックスに取り込み、langchainエコシステムに統合する際に、エンベッディングが互換性を持つかどうかを心配する必要はありません。

> Because Marqo include its own inference your documents can have a mix of text and images, you can bring Marqo indexes with data from your other systems into the langchain ecosystem without having to worry about your embeddings being compatible.

Marqoのデプロイは柔軟に対応できます。私たちのDockerイメージを使って自分でセットアップすることも、[マネージドクラウドオファリングについてお問い合わせいただくことも可能です！](https://www.marqo.ai/pricing)

> Deployment of Marqo is flexible, you can get started yourself with our docker image or [contact us about our managed cloud offering!](https://www.marqo.ai/pricing)

MarqoをローカルでDockerイメージを使用して実行するには、[こちらのガイドをご覧ください。](https://docs.marqo.ai/latest/)

> To run Marqo locally with our docker image, [see our getting started.](https://docs.marqo.ai/latest/)

## Installation and Setup | インストールとセットアップ

* `pip install marqo`でPython SDKをインストールしてください
  > Install the Python SDK with `pip install marqo`

## Wrappers | ラッパー

### VectorStore | VectorStore

Marqoインデックスをvectorstoreフレームワーク内で使用できるようにするラッパーが存在します。Marqoでは、埋め込みを生成するためのさまざまなモデルを選択でき、いくつかの前処理設定を公開しています。

> There exists a wrapper around Marqo indexes, allowing you to use them within the vectorstore framework. Marqo lets you select from a range of models for generating embeddings and exposes some preprocessing configurations.

Marqoのベクターストアは、画像とテキストが混在する既存のマルチモデルインデックスでも使用できます。詳細については、[当社のドキュメント](https://docs.marqo.ai/latest/#multi-modal-and-cross-modal-search)を参照してください。ただし、既存のマルチモデルインデックスでMarqoベクターストアをインスタンス化すると、langchainベクターストアの`add_texts`メソッドを使用して新しいドキュメントを追加する機能が無効になることに注意してください。

> The Marqo vectorstore can also work with existing multimodel indexes where your documents have a mix of images and text, for more information refer to [our documentation](https://docs.marqo.ai/latest/#multi-modal-and-cross-modal-search). Note that instaniating the Marqo vectorstore with an existing multimodal index will disable the ability to add any new documents to it via the langchain vectorstore `add_texts` method.

このベクトルストアをインポートするには：

> To import this vectorstore:

```python
from langchain.vectorstores import Marqo
```

Marqoラッパーとその独自の機能の詳細なウォークスルーについては、[このノートブック](/docs/integrations/vectorstores/marqo)をご覧ください。

> For a more detailed walkthrough of the Marqo wrapper and some of its unique features, see [this notebook](/docs/integrations/vectorstores/marqo)
