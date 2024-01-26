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
