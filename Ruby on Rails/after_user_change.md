## 新しいユーザを作成してから詰まったこと

こんなことはそうそうないのかもしれないが、諸事情あって、PCにゲストユーザを作成して、新しい環境を作った。

新環境で既存のRailsプロジェクトを立ち上げようとしたところ、いくつか詰まった点があったため、書き残しておく。

間違った方法でゴリ押してしまった点もあるかもしれないので要復習です。

### Git clone

環境構築で詰まったことはおいておいて、それ以降の話。

`git clone`で既存のプロジェクトを引っ張ってこようとしたら、パスワードの認証はもう廃止したからアクセストークン発行しておくんなましって言われた。

これは前にも言われたことがあるので、githubのヘルプページに載っている手順でアクセストークンを発行、`git clone` するとuser_nameとpasswordを聞かれるので、passwordの
ところに先ほど作成したアクセストークンを入力すればcloneはできる。

### 作業環境の再構築

git cloneができたので、とりあえず`bin/rails s`でサーバーを立ち上げてみる

→migrationしておくれ、というエラーが出た。db関連は引き継がれないのね。`bin/rails db:migration`で解決

もう一回サーバを立ち上げるが、今度はwebpacker関連のエラー。application.jsにimportで書いたbootstrapやpopperjsなど、yarn installした子たちのことかな。

`bin/yarn add bootstrap` / `bin/yarn add @popperjs/core`

これでhttp://localhost:3000 にアクセスすれば完了かな。

### credentialsなどの秘密ファイルの引き継ぎ

adminログインの画面で、ちっともログインできない。`bin/rails credentials:show`も使えない。

masterkeyなどの秘密情報はおそらく引き継がれていないため、なんとかしないといけない。

どうにか引き継げる方法があるか探したが、自分の調査力不足で見つけられず、master.keyとcredentials.yml.encのファイルは泣く泣く作り直すことにした。

configディレクトリの配下を見てみると、master.keyはない。

```
EDITOR=vim bin/rails credentials:edit
```

そうすると

```
create  config/master.key
```
と、master.keyを発行してもらえた。

`bin/rails credentials:show`しようとすると、エラーが出た。ざっくり要約すると「credentials.yml.encとmaster.keyが合わないよ」みたいな感じだった。

git cloneで引っ張ってきてしまったcredentials.yml.encは前の環境のmaster.keyで開くものだから当然か。

もとのcredentials.yml.encはrmした　→ これ、前の環境でまたプロジェクト立ち上げる時に詰むのでは。renameしてバックアップを取っておくか、もう初めからバージョン管理に載せないような
工夫がいるかもしれない。

とりあえず、新しくcredentials.yml.encを作成するべく

```
EDITOR=vim bin/rails credentials:edit
```

これでdb/seeds.rbできちんとadminユーザ情報を読み込めるように、credentialsの中身を書いておく。

```
bin/rails db:seed
```

これで一応、既存プロジェクトを立ち上げてadminでログインすることはできた。



