# Vearch | Vearch

[Vearch](https://github.com/vearch/vearch)は、ディープラーニングベクトルの効率的な類似性検索のためのスケーラブルな分散システムです。

> [Vearch](https://github.com/vearch/vearch) is a scalable distributed system for efficient similarity search of deep learning vectors.

# Installation and Setup | インストールとセットアップ

Vearch Python SDKを使うと、vearchをローカルで使用できます。Vearch Python SDKは、`pip install vearch`コマンドで簡単にインストール可能です。

> Vearch Python SDK enables vearch to use locally. Vearch python sdk can be installed easily by pip install vearch.

# Vectorstore | ベクターストア

Vearchはベクターストアとしても使用できます。詳細は[このノートブック](/docs/integrations/vectorstores/vearch)に記載されています。

> Vearch also can used as vectorstore. Most detalis in [this notebook](/docs/integrations/vectorstores/vearch)

```python
from langchain.vectorstores import Vearch
```
