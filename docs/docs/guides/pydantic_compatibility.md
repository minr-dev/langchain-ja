# Pydantic compatibility | Pydanticの互換性

* Pydantic v2は2023年6月にリリースされました（https://docs.pydantic.dev/2.0/blog/pydantic-v2-final/）
  > Pydantic v2 was released in June, 2023 (https://docs.pydantic.dev/2.0/blog/pydantic-v2-final/)
* v2には多数の破壊的変更が含まれています（https://docs.pydantic.dev/2.0/migration/）
  > v2 contains has a number of breaking changes (https://docs.pydantic.dev/2.0/migration/)
* Pydantic v2とv1は同じパッケージ名の下にありますので、両バージョンを同時にインストールすることはできません
  > Pydantic v2 and v1 are under the same package name, so both versions cannot be installed at the same time

## LangChain Pydantic migration plan | LangChain Pydantic 移行計画

`langchain>=0.0.267` 以降、LangChain はユーザーが Pydantic V1 または V2 のいずれかをインストールできるようになります。

> As of `langchain>=0.0.267`, LangChain will allow users to install either Pydantic V1 or V2.

* 内部的にLangChainは、[V1を使用し続けます](https://docs.pydantic.dev/latest/migration/#continue-using-pydantic-v1-features)。
  > Internally LangChain will continue to [use V1](https://docs.pydantic.dev/latest/migration/#continue-using-pydantic-v1-features).
* この期間中、ユーザーはpydanticのバージョンをv1に固定して互換性のない変更を避けることができます。または、LangChainのコードでv1とv2を混在させないように注意しながら（下記参照）、コード全体にわたってpydantic v2を使用した部分的な移行を行うことができます。
  > During this time, users can pin their pydantic version to v1 to avoid breaking changes, or start a partial
  > migration using pydantic v2 throughout their code, but avoiding mixing v1 and v2 code for LangChain (see below).

ユーザーは、pydantic v1に固定して、LangChainが内部的にv2に移行した後に一度にコードをアップグレードすることもできますし、v2への部分的な移行を開始することもできます。ただし、LangChain用のv1とv2のコードを混在させないように注意する必要があります。

> User can either pin to pydantic v1, and upgrade their code in one go once LangChain has migrated to v2 internally, or they can start a partial migration to v2, but must avoid mixing v1 and v2 code for LangChain.

以下に、継承の場合とLangChainにオブジェクトを渡す場合に、pydantic v1とv2のコードを混在させないようにする方法を示す二つの例を示します。

> Below are two examples of showing how to avoid mixing pydantic v1 and v2 code in
> the case of inheritance and in the case of passing objects to LangChain.

**例1: 継承を通じて拡張する**

> **Example 1: Extending via inheritance**

**はい**

> **YES**

```python
from pydantic.v1 import root_validator, validator

class CustomTool(BaseTool): # BaseTool is v1 code
    x: int = Field(default=1)

    def _run(*args, **kwargs):
        return "hello"

    @validator('x') # v1 code
    @classmethod
    def validate_x(cls, x: int) -> int:
        return 1
    

CustomTool(
    name='custom_tool',
    description="hello",
    x=1,
)
```

Pydantic v2のプリミティブとPydantic v1のプリミティブを混在させると、分かりにくいエラーが発生する可能性があります

> Mixing Pydantic v2 primitives with Pydantic v1 primitives can raise cryptic errors

**いいえ**

> **NO**

```python
from pydantic import Field, field_validator # pydantic v2

class CustomTool(BaseTool): # BaseTool is v1 code
    x: int = Field(default=1)

    def _run(*args, **kwargs):
        return "hello"

    @field_validator('x') # v2 code
    @classmethod
    def validate_x(cls, x: int) -> int:
        return 1
    

CustomTool( 
    name='custom_tool',
    description="hello",
    x=1,
)
```

**例2: オブジェクトをLangChainに渡す**

> **Example 2: Passing objects to LangChain**

**はい**

> **YES**

```python
from langchain_core.tools import Tool
from pydantic.v1 import BaseModel, Field # <-- Uses v1 namespace

class CalculatorInput(BaseModel):
    question: str = Field()

Tool.from_function( # <-- tool uses v1 namespace
    func=lambda question: 'hello',
    name="Calculator",
    description="useful for when you need to answer questions about math",
    args_schema=CalculatorInput
)
```

**いいえ**

> **NO**

```python
from langchain_core.tools import Tool
from pydantic import BaseModel, Field # <-- Uses v2 namespace

class CalculatorInput(BaseModel):
    question: str = Field()

Tool.from_function( # <-- tool uses v1 namespace
    func=lambda question: 'hello',
    name="Calculator",
    description="useful for when you need to answer questions about math",
    args_schema=CalculatorInput
)
```
