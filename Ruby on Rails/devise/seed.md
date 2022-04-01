## デフォルトadminをseeds.rbから作成

deviseで認証周りを実装する際、たとえばブログの管理者など、adminを画面上から新規に登録するのではなく、初期値として持たせていたい時もある（かもしれない）

そんな時には、db/seeds.rbに初期値を書きこんで実行することでadminの初期値を持たせることができる。

``` rb
# db/seeds.rb
# 初期値を入れるのはadminモデルと仮定

admins = Admin.new(:email => 'hogehoge@hoge', :password => 'hogehoge')
admins.save!
```

seeds.rbに必要な情報を入力することができたら

```
$ bin/rails db:seed
```

これで問題なく実行できれば、adminモデルに初期値が入る。

`db:seed`を複数回実行してしまうと、その都度レコードが追加されてしまう。最初に追加したマスタデータを更新したいような時には

```
$ bin/rails db:seed:replant
```
を実行。これを実行すると、一度レコードを削除して、あらためて`db:seed`が実行される。

### ちょっとまった

seeds.rbってgitの管理下に入っちゃうから、初期値のパスワードとかも公開されちゃうわけですか。

seeds.rbの中から環境変数化したメールやパスを読み込むようにしておいた方がいい？？

### 解決策

結局、credentials.yml.encにemailやpasswordの情報を書き込んで、seeds.rbからアクセスできるようにした。

credentialsに関しては[こちら](https://github.com/yuseipen0716/TIL/blob/main/Ruby%20on%20Rails/credentials.md)にまとめてある。

```
# credentials.yml.enc

admin:
  email: hogehoge@hoge
  password: hogehoge
```

``` rb
# config.seeds.rb

EMAIL = Rails.application.credentials.admin[:email]
PASSWORD = Rails.application.credentials.admin[:password]

admins = Admin.new(:email => EMAIL, :password => PASSWORD)
admins.save!
```




