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

## 【反省】お守りトランザクションを書いていた
複数のSQLを発行する場合は`ActiveRecord::Base.transaction do end`で囲む、みたいな認識でいたため、意味のないtransactionブロックを書いてしまっていたケースがあったため反省。

```ruby
def save_user(attrs)
  user = User.new(attrs)

  ActiveRecord::Base.transaction do
    if user.save && CreateOtherModelRecordUsecase.perform(attrs)
      { status: :ok, message: 'ユーザーの作成に成功しました' }
    else
      { status: :bad_request, message: 'errorが発生しました。' }
    end
  end
end

# CreateOtherModelRecordUsecase
class CreateOtherModelRecordUsecase
  def self.perform(attrs)
    # attrsを使用してOtherModelのrecordをbuildする処理（省略）

    record.save
  end
end
```

例えば、上記のようなtransactionだと、`user.save`に失敗した場合にはuserレコードは作成されず、status: :bad_requestが返されるが、`user.save`に成功したが、CreateOtherModelUsecase.performでこけた場合に、Rollbackは起こらず、elseの分岐に入り、status: :bad_requestを返すが、userレコードはDBに残ってしまうように思う。

#### 改善案
```ruby
def save_user(attrs)
  user = User.new(attrs)

  ActiveRecord::Base.transaction do
    if user.save && CreateOtherModelRecordUsecase.perform!(attrs)
      { status: :ok, message: 'ユーザーの作成に成功しました' }
    else
      { status: :bad_request, message: 'errorが発生しました。' }
    end
  end
rescue CustomOtherModelError => e
  Rails.logger.error("エラーが発生しました: #{e.message}")
  { status: :bad_request, message: 'errorが発生しました。' }
end

# CreateOtherModelRecordUsecase
class CreateOtherModelRecordUsecase
  def self.perform(attrs)
    # attrsを使用してOtherModelのrecordをbuildする処理（省略）

    record.save
  end

  # 保存に失敗したら例外をスロー
  def self.perform!(attrs)
    # attrsを使用してOtherModelのrecordをbuildする処理（省略）

    return true if record.save

    raise CustomOtherModelError # カスタムエラークラスの例外をスロー
  end
end
```

このようにすれば、OtherModelのレコードを作成しようとしてエラーが発生した場合には、トランザクションが自動的にロールバックされ、なおかつカスタムエラークラスを捕捉して、status: :bad_requestのレスポンスも返すことができるはず。
