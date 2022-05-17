## migration関連あれこれ

### migration down→upにしたい

`rails db:migrate:status`を叩いてdown→upにしたいMigration IDをコピー

```
$ (bundle exec) rails db:migrate:up VERSION=<Migration ID>
```

### [error] ActiveRecord::StatementInvalid: Mysql2::Error: Duplicate column name...

migrationしようと思ったらカラムが重複していますというエラー

この前段階でmigrationを失敗していたのが原因かもしれないが、とりあえず解決策を調べると、

1. 該当migrationファイルのchangeメソッド内の記述をコメントアウト
2. `rails db:migrate` で空マイグレーション
3. 1でつけたコメントアウトを外す
4. `rails db:migrate`
5. OK！ 念のため、`rails db:migrate:status`で確認
