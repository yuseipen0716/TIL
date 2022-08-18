## ActiveRecordでtransactionを使用する

```
ActiveRecord::Base.transaction do
```

というようなブロックで囲んだ中にdbにデータを格納する処理を書いているような実装を見かけた。

トランザクションとはそもそも何か、どうして必要かなども含め、復習していく。

エラーハンドリングする位置も気を付けないといけない。
