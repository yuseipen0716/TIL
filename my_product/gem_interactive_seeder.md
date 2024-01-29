# Railsで使用できる、対話式のseederを開発したい

まずは、いきなりgemではなく、仕事の方で必要とされている上記の機能のみをrakeタスクか何かで作ってみる。

## 対話式のseedingを行うためのrakeタスク
一旦、モデル名をプロンプトで聞いて、入力されたモデル名を利用して対話式のseeding処理を進めていく。

```ruby
namespace :record do
  desc "Modelを指定して、テストデータを作成するためのrakeタスク"
  task :generate => :environment do
    puts "> テストデータを作成したいモデル名を入力してください（例: User）"
    model_name = $stdin.gets.chomp

    # model_nameを利用して対話式のseedingを進める処理（これはservices等に切り出す）
  end
end

```

### やっていて感じた課題
- belongs_toでoptional:trueでない関連付けは事前に用意しておく必要があったり、has_manyの関連をどのタイミングで作成するかなどややむずかしい。
  - 事前に準備が必要なモデルであってもマスタデータが存在するものは新規でレコードを作成してはいけないので、そのあたりはidを選択するようなプロンプトにしないと事故りそう。
    - Gemにするような一般化するレベルなら、configファイルみたいなのを読み込んで、`exclude_models`みたいなのを設定しておかないといけないか。
  - belongs_toの準備は再帰的にprepare_relationsみたいな処理を回すが、has_manyの方は、`@params`ハッシュ内にattributesとして格納できれば良いので、もう少し考えないと。 
- formatやexampleの整備が大変。また、defaultのparamsを作成するのはどのようにしたらよいか。（Modelのcolumnsをeachで回せば、columnのdefaultバリューも取得できる。formatなどの指定がない場合はフォールバック的にcolumnのdefaultをparamsに入れたい気持ちはあるが。）
- 1度に複数のレコード（3人のUserとか）を作成したいときには弱いなぁ
- 複数のレコードを用意する場合は、seed_fuなどのpathを指定して特定のデータセットを作成するような形にした方が良いかも。

### 案
- 「データセットを選択してレコードを作成しますか？（y/n） nの場合は、対話的に個別レコードを作成します。」という選択肢を出し、yが選択された場合はseed_fuコマンドで実行可能なデータセットを選択してもらい、該当pathを付けてdb:seed_fuを走らせる。nの場合は、対話式で個別レコードを作成するような処理に移行。
