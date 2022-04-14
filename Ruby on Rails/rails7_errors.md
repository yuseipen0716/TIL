## Rails7を動かしていて遭遇したエラー

EverydayRailsをサンプルコードを見ながらあ進めている。system specのところで、テストしようと思ったらasset pipelineのエラーが出て困った

調べてみると、著者様のQiita記事に詰まったところがまとめてあった。

[Rails 7.0 + Ruby 3.1でゼロからアプリを作ってみたときにハマったところあれこれ](https://qiita.com/jnchito/items/5c41a7031404c313da1f)

本書にある通りにセットアップをすすめてみれば問題なく動いたので、ここにもメモしておく。各々の詳細についても調べていかないとね。

### セットアップ手順

環境

- Rails7系
- Ruby3.1.0
- Git
- Yarn

- 必要であればgemインストール
  - `bundle install`
- データベースのセットアップ等
  - `bin/setup`
- JavaScriptパッケージのインストール
  - `yarn install`
- foreman gemのインストール
  - `gem install foreman`
- サーバの起動
  - `bin/dev`
- 必要であれば、rspecのコマンドの短縮
  - `bundle binstubs rspec-core`
