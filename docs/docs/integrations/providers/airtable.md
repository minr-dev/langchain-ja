# Airtable | Airtable

> [Airtable](https://en.wikipedia.org/wiki/Airtable)はクラウド協働サービスです。`Airtable`は、スプレッドシートとデータベースのハイブリッドで、データベースの特徴をスプレッドシートに適用したものです。Airtableのテーブル内のフィールドはスプレッドシートのセルに似ていますが、'チェックボックス'、'電話番号'、'ドロップダウンリスト'などのタイプがあり、画像などのファイル添付を参照できます。
>
> > [Airtable](https://en.wikipedia.org/wiki/Airtable) is a cloud collaboration service.
> > `Airtable` is a spreadsheet-database hybrid, with the features of a database but applied to a spreadsheet.
> > The fields in an Airtable table are similar to cells in a spreadsheet, but have types such as 'checkbox',
> > 'phone number', and 'drop-down list', and can reference file attachments like images.

> ユーザーはデータベースを作成し、カラムの種類を設定し、レコードを追加し、テーブルを相互にリンクさせ、協力してレコードをソートし、外部ウェブサイトにビューを公開することができます。
>
> > Users can create a database, set up column types, add records, link tables to one another, collaborate, sort records
> > and publish views to external websites.

## Installation and Setup | インストールとセットアップ

```bash
pip install pyairtable
```

* [APIキー](https://support.airtable.com/docs/creating-and-using-api-keys-and-access-tokens)を取得してください。
  > Get your [API key](https://support.airtable.com/docs/creating-and-using-api-keys-and-access-tokens).
* あなたのベースの[IDを取得してください](https://airtable.com/developers/web/api/introduction)。
  > Get the [ID of your base](https://airtable.com/developers/web/api/introduction).
* [テーブルのURLからテーブルIDを取得する](https://www.highviewapps.com/kb/where-can-i-find-the-airtable-base-id-and-table-id/#:~:text=Both%20the%20Airtable%20Base%20ID,URL%20that%20begins%20with%20tbl)についてはこちらをご覧ください。
  > Get the [table ID from the table url](https://www.highviewapps.com/kb/where-can-i-find-the-airtable-base-id-and-table-id/#:~:text=Both%20the%20Airtable%20Base%20ID,URL%20that%20begins%20with%20tbl).

## Document Loader | ドキュメントローダー

```python
from langchain.document_loaders import AirtableLoader
```

[例](/docs/integrations/document_loaders/airtable)を見てください。

> See an [example](/docs/integrations/document_loaders/airtable).
