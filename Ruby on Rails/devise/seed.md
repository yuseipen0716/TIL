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

### ちょっとまった

seeds.rbってgitの管理下に入っちゃうから、初期値のパスワードとかも公開されちゃうわけですか。

seeds.rbの中から環境変数化したメールやパスを読み込むようにしておいた方がいい？？
