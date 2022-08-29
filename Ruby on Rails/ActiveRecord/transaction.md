## ActiveRecordでtransactionを使用する

```
ActiveRecord::Base.transaction do
```

というようなブロックで囲んだ中にdbにデータを格納する処理を書いているような実装を見かけた。

トランザクションとはそもそも何か、どうして必要かなども含め、復習していく。

エラーハンドリングする位置も気を付けないといけない。

[【翻訳】ActiveRecordにおける、ネストしたトランザクションの落とし穴](https://qiita.com/jnchito/items/930575c18679a5dbe1a0)

この記事、は読んでおいた方がよさそう。

自前でtransaction処理を書く際には

```ruby
ActiveRecord::Base.transaction(joinable: false, requires_new: true) do
  # inner code
end
```

とするのがよさそう。
