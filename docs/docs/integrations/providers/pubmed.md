# PubMed | PubMed

# PubMed | PubMed

> [PubMed®](https://pubmed.ncbi.nlm.nih.gov/)は、`The National Center for Biotechnology Information, National Library of Medicine`が提供するサービスであり、`MEDLINE`、生命科学のジャーナル、オンラインの書籍からの3500万以上の生物医学文献の引用を含んでいます。引用には、`PubMed Central`や出版社のウェブサイトからの全文コンテンツへのリンクが含まれることがあります。
>
> > [PubMed®](https://pubmed.ncbi.nlm.nih.gov/) by `The National Center for Biotechnology Information, National Library of Medicine`
> > comprises more than 35 million citations for biomedical literature from `MEDLINE`, life science journals, and online books.
> > Citations may include links to full text content from `PubMed Central` and publisher web sites.

## Setup | セットアップ

Pythonパッケージをインストールする必要があります。

> You need to install a python package.

```bash
pip install xmltodict
```

### Retriever | リトリーバー

[使用例](/docs/integrations/retrievers/pubmed)をご覧ください。

> See a [usage example](/docs/integrations/retrievers/pubmed).

```python
from langchain.retrievers import PubMedRetriever
```

### Document Loader | ドキュメントローダー

[使用例](/docs/integrations/document_loaders/pubmed)をご覧ください。

> See a [usage example](/docs/integrations/document_loaders/pubmed).

```python
from langchain.document_loaders import PubMedLoader
```
