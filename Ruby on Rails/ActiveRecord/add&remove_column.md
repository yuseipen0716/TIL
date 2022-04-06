## [ActiveRecord]カラムの追加

### モデルに新しくカラムを追加したい時の手順

```
$ bin/rails generate migration Addカラム名Toテーブル名 カラム名:データ型
```
↑を実行すると、migrationファイルが追加されるので、↓を実行

```
$ rails db:migrate
```

OK

### カラムを削除したい時の手順

```
$ rails generate migration Removeカラム名Fromテーブル名 カラム名:データ型
```
↑を実行すると、migrationファイルが追加されるので、↓を実行

```
$ rails db:migrate
```

