# Security | セキュリティ

LangChainは、ローカルおよびリモートのファイルシステム、API、データベースなど、さまざまな外部リソースとの広範な統合エコシステムを有しています。これらの統合により、開発者はLLMの力を活用し、外部リソースにアクセスして対話し、それらを操作することができる多様なアプリケーションを作成することが可能です。

> LangChain has a large ecosystem of integrations with various external resources like local and remote file systems, APIs and databases. These integrations allow developers to create versatile applications that combine the power of LLMs with the ability to access, interact with and manipulate external resources.

## Best Practices | ベストプラクティス

そのようなアプリケーションを構築する際、開発者は良いセキュリティ慣行に従うべきです。

> When building such applications developers should remember to follow good security practices:

* [**権限を制限する**](https://ja.wikipedia.org/wiki/最小権限の原則)：権限はアプリケーションの必要に応じて具体的に設定してください。広範囲にわたる権限や過剰な権限を付与することは、重大なセキュリティの脆弱性を引き起こす可能性があります。このような脆弱性を避けるためには、読み取り専用の認証情報の使用、機密性の高いリソースへのアクセスを禁止すること、サンドボックス技術（コンテナ内で実行するなど）の使用など、アプリケーションに適した方法を検討してください。
  > [**Limit Permissions**](https://en.wikipedia.org/wiki/Principle_of_least_privilege): Scope permissions specifically to the application's need. Granting broad or excessive permissions can introduce significant security vulnerabilities. To avoid such vulnerabilities, consider using read-only credentials, disallowing access to sensitive resources, using sandboxing techniques (such as running inside a container), etc. as appropriate for your application.
* **潜在的な誤用に備える**: 人間が間違いを犯すように、大規模言語モデル（LLMs）も間違いを犯すことがあります。システムアクセスや認証情報が、それらに割り当てられた権限の範囲内であらゆる方法で使用され得ると常に想定してください。例えば、データベースの認証情報がデータ削除を許可している場合、その認証情報を使用可能なLLMが実際にデータを削除するかもしれないと想定するのが最も安全です。
  > **Anticipate Potential Misuse**: Just as humans can err, so can Large Language Models (LLMs). Always assume that any system access or credentials may be used in any way allowed by the permissions they are assigned. For example, if a pair of database credentials allows deleting data, it’s safest to assume that any LLM able to use those credentials may in fact delete data.
* [**ディフェンス・イン・デプス**](https://ja.wikipedia.org/wiki/防御の深さ_\(コンピューティング\)): どのセキュリティ技術も完璧ではありません。細かな調整と良いチェーン設計によって、大規模言語モデル（LLM）が誤りを犯す可能性を減らすことはできますが、完全には排除できません。複数の層を重ねたセキュリティアプローチを組み合わせることが、任意の単一の防御層に依存するよりも、セキュリティを確保する上で最善です。例えば、LLMが明示的に使用することを意図されたデータのみにアクセスできるように、読み取り専用の権限とサンドボックスを併用します。
  > [**Defense in Depth**](https://en.wikipedia.org/wiki/Defense_in_depth_\(computing\)): No security technique is perfect. Fine-tuning and good chain design can reduce, but not eliminate, the odds that a Large Language Model (LLM) may make a mistake. It’s best to combine multiple layered security approaches rather than relying on any single layer of defense to ensure security. For example: use both read-only permissions and sandboxing to ensure that LLMs are only able to access data that is explicitly meant for them to use.

それを行わないリスクには以下のものが含まれますが、これに限りません：

> Risks of not doing so include, but are not limited to:

* データの破損または損失。
  > Data corruption or loss.
* 機密情報への不正アクセス。
  > Unauthorized access to confidential information.
* 重要なリソースのパフォーマンスや可用性が低下しています。
  > Compromised performance or availability of critical resources.

緩和戦略を含む事例シナリオ：

> Example scenarios with mitigation strategies:

* ユーザーは、ファイルシステムにアクセスできるエージェントに、削除してはならないファイルの削除や、機密情報を含むファイルの内容を読むよう依頼することがあります。これを緩和するために、エージェントが特定のディレクトリのみを使用し、読み書きが安全なファイルのみを読み書きできるように制限してください。さらに、エージェントをコンテナで実行することにより、サンドボックス化を進めることを検討してください。
  > A user may ask an agent with access to the file system to delete files that should not be deleted or read the content of files that contain sensitive information. To mitigate, limit the agent to only use a specific directory and only allow it to read or write files that are safe to read or write. Consider further sandboxing the agent by running it in a container.
* ユーザーは、外部APIへの書き込み権限を持つエージェントに対して、そのAPIに悪意のあるデータを書き込むか、またはデータを削除するよう依頼することがあります。これを緩和するためには、エージェントに読み取り専用のAPIキーを与えるか、あるいはそのような悪用に対して既に耐性があるエンドポイントのみを使用するように制限することが有効です。
  > A user may ask an agent with write access to an external API to write malicious data to the API, or delete data from that API. To mitigate, give the agent read-only API keys, or limit it to only use endpoints that are already resistant to such misuse.
* ユーザーは、データベースにアクセスできるエージェントにテーブルの削除やスキーマの変更を依頼することがあります。これを緩和するために、エージェントがアクセスする必要があるテーブルにのみクレデンシャルの範囲を限定し、読み取り専用のクレデンシャルを発行することを検討してください。
  > A user may ask an agent with access to a database to drop a table or mutate the schema. To mitigate, scope the credentials to only the tables that the agent needs to access and consider issuing READ-ONLY credentials.

ファイルシステム、API、データベースなどの外部リソースにアクセスするアプリケーションを構築する際には、アプリケーションをどのように設計し、セキュリティを確保するかを最適に決定するために、企業のセキュリティチームに相談することを検討してください。

> If you're building applications that access external resources like file systems, APIs
> or databases, consider speaking with your company's security team to determine how to best
> design and secure your applications.

## Reporting a Vulnerability | 脆弱性の報告

セキュリティの脆弱性は、迅速にトリアージされ、必要に応じて対応されるよう、security@langchain.dev 宛にメールで報告してください。

> Please report security vulnerabilities by email to security@langchain.dev. This will ensure the issue is promptly triaged and acted upon as needed.

## Enterprise solutions | エンタープライズソリューション

LangChainは、追加のセキュリティ要件を持つお客様に対してエンタープライズソリューションを提供することがあります。sales@langchain.devまでお問い合わせください。

> LangChain may offer enterprise solutions for customers who have additional security
> requirements. Please contact us at sales@langchain.dev.
