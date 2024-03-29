## Rackについてのメモ

### Rackとは
WebアプリケーションサーバとWebアプリケーションフレームワークの中間に存在する。

ひとことで言うと

> RubyでWebサーバを立ち上げるためのインターフェース(引用: [Rackとは何か](https://qiita.com/k0kubun/items/248395f68164b52aec4a))

Rack登場前はWebアプリケーションサーバとWebアプリケーションフレームワークの中間に位置するものはなく、両者の結合が密であった。

そのため、あるWebサーバはあるWebフレームワークだと使えない、なんてことも起こっていたらしい。

Rackが開発、実装され、両者の中間に存在することで、それぞれの結合が疎になり、様々なフレームワークとWebサーバの組み合わせを選択できるようになり、開発者にとって
より良い組み合わせを選択できるようになった。

### Rackミドルウェア
RackにはRack層からフレームワーク層に到達するまでの間に処理を挟むことができるRackミドルウェアと呼ばれる機構がある。

これにより、独自の処理を差し込んだり、よく使われる機能がライブラリ化されるようになった。

### Rackに必要なもの
Rackは非常にシンプルなインターフェースを定義している。（Rack protocol）Rackアプリケーションやフレームワークはこの規約に則ったインターフェースを定義する必要がある。

- Rackアプリケーションは引数を1つとり、3つの値を返す`call`メソッドを呼び出すことができるオブジェクトであると定義されている。
- `call`メソッドがとる引数は慣習的にenvあるいは、environmentと命名する必要がある。
- `call`メソッドは次の3つの値を配列型で戻り値を返す必要がある。
  - HTTPのステータスコードを表す数値オブジェクト
  - HTTPヘッダーを表すハッシュオブジェクト
  - レスポンスボディとなる文字列を含んだ配列風オブジェクト

以上の規約に則って端的な疑似コードを書くとこんな感じ。
```ruby
def call(env)
  [status, headers, body]
end
```

Rackが利用するエントリーポイント用のファイル名として、`config.ru`というファイルを使用する。

最小のRackアプリケーションを作成するために、パーフェクトRORに例が載っていた。
```ruby
# config.ru
require 'rack'
require_relative 'app'

run App.new
```

RackはRack::Builder DSLという独自のDSLを提供しており、`rackup`を叩くと、rackup configに指定したファイルをRack::Builder DSLとして読み込み、設定されたWebサーバが立ち上がる。

rackup configの指定がないと、カレントディレクトリのconfig.ruが読み込まれる。`run`だけは指定が必要。

callメソッドに渡されるenvという引数はHTTPリクエストとなっている。(パーフェクトROR p.133参照)

そのため、Rackの仕組みとしてはHTTPリクエストとしてわたってくる値をcallメソッドで受け、HTTPのレスポンスを返している。

> Rackのcallメソッドで定義されているインターフェイスというのはHTTPリクエストとHTTPレスポンスに対する取り決めをRubyの基本的なクラスで規定しているということ






### 参考
- パーフェクトRuby on Rails
