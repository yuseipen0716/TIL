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


### マイグレーションファイルを削除したいとき

```
bundle exec rails db:migration:status
```

で削除したいマイグレーションファイルのstatusがdownになっていることを確認。

downになっていなければ、上記の方法でdownに

statusがdownになっていることが確認できれば、ファイルを削除してOK

### db/schema.rbに想定外のdiffがある

db/schema.rbの内容は、現段階でのDBの状況を反映するため、developブランチやmaster, mainブランチと記載内容が異なる場合がある。

ただ、schema.rbのファイルの内容に関して自分たちは何もできず、Railsがよしなにやってくれるので、内容を確認してcommitするしかない。


