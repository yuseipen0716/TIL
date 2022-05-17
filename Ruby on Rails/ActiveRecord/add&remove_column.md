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

