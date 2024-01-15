# Custom example selector | カスタムサンプルセレクター

このチュートリアルでは、与えられた例のリストからランダムに例を選択するカスタムの例選択器を作成します。

> In this tutorial, we'll create a custom example selector that selects examples randomly from a given list of examples.

`ExampleSelector`は、二つのメソッドを実装しなければなりません：

> An `ExampleSelector` must implement two methods:

1. `add_example` メソッドは、例を受け取って ExampleSelector に追加します
   > An `add_example` method which takes in an example and adds it into the ExampleSelector
2. `select_examples`メソッドは、ユーザー入力を意図した入力変数を受け取り、数ショットプロンプトで使用する例のリストを返します。
   > A `select_examples` method which takes in input variables (which are meant to be user input) and returns a list of examples to use in the few-shot prompt.

ランダムに2つの例を選ぶだけのカスタム`ExampleSelector`を実装しましょう。

> Let's implement a custom `ExampleSelector` that just selects two examples at random.

**注意:**
LangChainでサポートされている現在の例示セレクターの実装を[こちら](/docs/modules/model_io/prompts/example_selectors/)でご覧ください。

> **Note:**
> Take a look at the current set of example selector implementations supported in LangChain [here](/docs/modules/model_io/prompts/example_selectors/).

<!-- TODO(shreya): Add the correct link. -->

## Implement custom example selector | カスタム例のセレクタを実装する

```python
from langchain.prompts.example_selector.base import BaseExampleSelector
from typing import Dict, List
import numpy as np


class CustomExampleSelector(BaseExampleSelector):
    
    def __init__(self, examples: List[Dict[str, str]]):
        self.examples = examples
    
    def add_example(self, example: Dict[str, str]) -> None:
        """Add new example to store for a key."""
        self.examples.append(example)

    def select_examples(self, input_variables: Dict[str, str]) -> List[dict]:
        """Select which examples to use based on the inputs."""
        return np.random.choice(self.examples, size=2, replace=False)

```

## Use custom example selector | カスタム例のセレクタを使用する

```python

examples = [
    {"foo": "1"},
    {"foo": "2"},
    {"foo": "3"}
]

# Initialize example selector.
example_selector = CustomExampleSelector(examples)

# Select examples
example_selector.select_examples({"foo": "foo"})
# -> array([{'foo': '2'}, {'foo': '3'}], dtype=object)

# Add new example to the set of examples
example_selector.add_example({"foo": "4"})
example_selector.examples
# -> [{'foo': '1'}, {'foo': '2'}, {'foo': '3'}, {'foo': '4'}]

# Select examples
example_selector.select_examples({"foo": "foo"})
# -> array([{'foo': '1'}, {'foo': '4'}], dtype=object)
```
