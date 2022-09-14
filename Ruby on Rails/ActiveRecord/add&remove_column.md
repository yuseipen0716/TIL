## [ActiveRecord]カラムの追加

### モデルに新しくカラムを追加したい時の手順

```
$ bin/rails generate migration Add<column_name>To<table_name> <column_name>:<data_type>
```
↑を実行すると、migrationファイルが追加されるので、↓を実行

```
$ rails db:migrate
```

OK

カラムを複数追加するときは、こんなふうに書いているみたい。

```
$ rails generate migration AddDetailsToTitles price:integer author:string
```

↓

``` ruby 
class AddDetailsToTitles < ActiveRecord::Migration[5.1]
  def change
    add_column :<table名>, :<追加するcolumn名>, :<データ型>, <規約などを記述→>null: false, default: false, comment: "説明文書いてもよい"
  end
end
```

↓

`rails db:migrate`


### カラムを削除したい時の手順

```
$ rails generate migration Remove<column_name>From<table_name> <column_name>:<data_type>
```
↑を実行すると、migrationファイルが追加されるので、↓を実行

```
$ rails db:migrate
```

### カラム追加時にデフォルト値を設定せずにNOT NULL制約を付けたい
NOT NULL制約をつけたカラムを追加したいが、既存のレコードが存在する場合、そのまま追加してしまうとエラーが出る。
( もともとのレコードは該当カラムのデータがNULLなため )

この場合、追加するカラムにデフォルト値を設定してあげればエラーも出ずに動くと思うが、今回はデフォルト値を設定しない方法を試した。

#### おおまかな手順
- NOT NULLを付けずに add_column
- <teble名>.update_all(<追加するカラム>: <設定する値>)
- change_column_nullメソッドでNOT NULLを追加

今回はマイグレーションファイルの中で、changeメソッドでなく、up, downメソッドを使用した。（[Railsガイドのリンクはこちら](https://railsguides.jp/active_record_migrations.html#up-down%E3%83%A1%E3%82%BD%E3%83%83%E3%83%89%E3%82%92%E4%BD%BF%E3%81%86)）

changeメソッドを使ってカラムの情報を変更する場合、changeメソッドの内容を適用し、もともと適用されていたものは取り消す操作が自動で行われる。

upメソッドでadd_columnなどを行われる場合はchangeメソッドが自動で行ってくれるrollbackの処理を自分で書かないといけない。

そこで、downメソッド内に、upメソッド内で書いた処理の逆の順番で、カラムの適用を取り消すような処理を書く。(upメソッド内でadd_columnしたときはdownメソッド内でremove_columnする)



> 参考: [Railsガイド マイグレーション](https://railsguides.jp/active_record_migrations.html)



