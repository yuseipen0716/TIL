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

### カラムを削除したい時の手順

```
$ rails generate migration Remove<column_name>From<table_name> <column_name>:<data_type>
```
↑を実行すると、migrationファイルが追加されるので、↓を実行

```
$ rails db:migrate
```

