## migration関連あれこれ

### migration down→upにしたい

`rails db:migrate:status`を叩いてdown→upにしたいMigration IDをコピー

migrationのVERSIONは`rails db:version`でも教えてくれる。

直前のやつならrails db:rollbackでもdownにできる。downになっている時ならmigrationファイルを編集したり、削除することができる。

```
$ (bundle exec) rails db:migrate:up VERSION=<Migration ID>
```

### [error] ActiveRecord::StatementInvalid: Mysql2::Error: Duplicate column name...

migrationしようと思ったらカラムが重複していますというエラー

この前段階でmigrationを失敗していたのが原因かもしれないが、とりあえず解決策を調べると、

1. 該当migrationファイルのchangeメソッド内の記述をコメントアウト
2. `rails db:migrate` で空マイグレーション
3. 1でつけたコメントアウトを外す
4. `rails db:migrate`
5. OK！ 念のため、`rails db:migrate:status`で確認


### マイグレーションファイルを削除したいとき

```
bundle exec rails db:migration:status
```

で削除したいマイグレーションファイルのstatusがdownになっていることを確認。

downになっていなければ、上記の方法でdownに

statusがdownになっていることが確認できれば、ファイルを削除してOK

migrationのコミットをresetやrevertした等の理由から、

```
up     20220726101239  ********** NO FILE **********
```
こんな状態で残ってしまった場合の解決法→ [【Ruby on Rails】 　NO FILEのmigrationファイル削除する](https://qiita.com/takeshi075/items/3e55615c9f10b3f8731e)

#### 簡単に手順をメモしておく
- `bin/rails db:migrate:status`でNO FILEとなっているmigrationのVERSIONをチェック(今回は例として`20220726101239`とする)
- `vi db/migrate/20220726101239_tmp.rb`で仮ファイルを作成する。(このファイルは後で消します)
- ```ruby
  class Tmp < ActiveRecord::Migration[5.1] # <- Railsのバージョンをここに記載
    def change
    end
  end
  ```
- `bin/rails db:migrate:status`で先ほどのNO FILE部分がTmpに代わっていることを確認
- `bin/rails db:migrate:down VERSION=20220726101239`でTmpのstatusをdownにする。念のため、`bin/rails db:migrate:status`で確認
- statusがdownになっているのを確認したら、`rm -rf db/migrate/20220726101239_tmp.rb`でtmpファイルを削除
- `bin/rails db:migrate:status`でNO FILEがなくなっているかどうかを確認。

以上


### db/schema.rbに想定外のdiffがある

db/schema.rbの内容は、現段階でのDBの状況を反映するため、developブランチやmaster, mainブランチと記載内容が異なる場合がある。

ただ、schema.rbのファイルの内容に関して自分たちは何もできず、Railsがよしなにやってくれるので、内容を確認してcommitするしかない。

---

### keyが長すぎだとMySQLに怒られたとき

カラムにindexを貼ろうとして、（今回はURLのカラム(string)にindexを貼りたかった）migrationファイルを作成し、db:migrateしたら怒られた

```
Mysql2::Error: Specified key was too long; max key length is 767 bytes: CREATE TABLE
```

keyが長すぎるんじゃ!767byte以内に収めろ、と言われている。

調べてみると、こちら（[MySQL5.6でActiveRecordのencodingがutf8mb4だとKey長すぎ問題の対応](https://blog.mothule.com/ruby/rails/active-record/ruby-rails-activerecord-fix-mysql56-encode-utf8mb4-key-too-length))の記事が出てきた。

原因としてはタイトルの通りである。RailsのActiveRecordにおけるstringはMySQLのVARCHAR(255)らしく、utf8mb4のencodingだと、MAXが1020bytesになってしまう(?)

手っ取り早い解決策としては

```ruby
class CreateAdControlledPages < ActiveRecord::Migration[5.1]
  def change
    create_table :ad_controlled_pages do |t|
      t.string :url, null: false, index: true, unique: true, limit: 191
      t.integer :control_type, default: 0, null: false

      t.timestamps
    end
  end
end
```
というように、limit、文字数制限を設けてあげること。767 / 4 = 191.75なので、191をMAXの文字数としてあげればいいみたい。

今回はこの方法を試したら、無事URLにindexを貼ることができました。あとは、どのカラムにindexを貼るかなどの検討も必要かもね。

↑↑のマイグレーションファイル、書き方間違えてた。

正しくは

```ruby
class CreateAdControlledPages < ActiveRecord::Migration[5.1]
  def change
    create_table :ad_controlled_pages do |t|
      t.string :url, null: false, limit: 191
      t.integer :control_type, default: 0, null: false

      t.timestamps
    end
    add_index :ad_controlled_pages, :url, unique: true
  end
end
```
こう。

最初の書き方だと、indexは貼れていたっぽいけど、unique制約が機能していなかった。


---
## カラムの重複で怒られた

rails db:migrateをして、migrationファイルの記述がまずかったようで、エラーがでた。（具体的には、カラムの追加はできたが、indexを貼るのに失敗したような感じ）

migrationファイルを修正して、再度rails db:migrateを実行したら「カラムが重複しているよ！」と怒られました。

DBの内容的にはカラム追加したところまでは完了しているのに、再度、同名のカラム追加とインデックス貼りをしてしまっていたので

### NOT NULL制約を後付け（または、なくしたい）







