## db:reset と db:migrate:resetの違い

開発中、別ブランチの動作テストをする必要があり、checkoutしたが、developブランチに最近mergeしたブランチでmigrationの追加等を行っていたため、データベースの不整合?が
起きた。

とりあえず、現行のデータをmysqldumpしておいて、`db:reset`することにした。どうやら、`db:migrate:reset`というのもあるらしく、調べた。

### rails db:reset

`rails db:reset`では、一旦databaseをdropして、schemaファイルをもとに、databaseを作り直す操作

### rails db:migrate:reset

`rails db:migrate:reset`では、databaseをdropしたあと、migrationファイルをもとにdatabaseを作り直す操作
