## いろいろメモ

個別にタイトルをつけてファイルを残すのが難しいものや、メモを書き残しておく。

### to_sqlで発行されるSQLクエリを確認する

``` ruby
@tv_articles = Category.find_by(slug: 'tv').articles.status_publish.order(published_at: 'DESC').limit(12)
```
たとえば、こんな風にして、カテゴリ別の新着記事を拾ってくる場合、どのようなSQLが発行されているか、確認したいときもある。

そんなときに`to_sql`メソッドを使用すると、発行されるクエリを確認することができる。

``` ruby
pry(main)> @tv_articles = Category.find_by(slug: 'tv').articles.status_publish.order(published_at: 'DESC').limit(12).to_sql
=> "SELECT  `articles`.* FROM `articles` INNER JOIN `article_categories` ON `articles`.`id` = `article_categories`.`article_id` WHERE `article_categories`.`category_id` = 20 AND `articles`.`status` = 0 ORDER BY `articles`.`published_at` DESC LIMIT 12"
```

### where.notの注意点

諸事情あって、

- nil
- 0
- 1

の値をもつカラムがあった。

nilのものと1のものを抽出したかったので、`where.not(column_name: 0)`としたが、`nil`のデータが取得できなかった。

[Railsガイド >> Active Record クエリインターフェイス >> 3.4NOT条件](https://railsguides.jp/active_record_querying.html#not%E6%9D%A1%E4%BB%B6:~:text=%E3%81%82%E3%82%8B%E3%82%AF%E3%82%A8%E3%83%AA%E3%81%AEnull%E8%A8%B1%E5%AE%B9%EF%BC%88nullable%EF%BC%89%E3%82%AB%E3%83%A9%E3%83%A0%E3%81%AB%E3%80%81%E9%9D%9Enil%E5%80%A4%E3%82%92%E6%8C%87%E5%AE%9A%E3%81%97%E3%81%9F%E3%83%8F%E3%83%83%E3%82%B7%E3%83%A5%E6%9D%A1%E4%BB%B6%E3%81%8C%E3%81%82%E3%82%8B%E5%A0%B4%E5%90%88%E3%80%81null%E8%A8%B1%E5%AE%B9%E3%82%AB%E3%83%A9%E3%83%A0%E3%81%ABnil%E5%80%A4%E3%82%92%E6%8C%81%E3%81%A4%E3%83%AC%E3%82%B3%E3%83%BC%E3%83%89%E3%81%AF%E8%BF%94%E3%81%95%E3%82%8C%E3%81%BE%E3%81%9B%E3%82%93%E3%80%82)

にあるように、

> あるクエリのnull許容（nullable）カラムに、非nil値を指定したハッシュ条件がある場合、null許容カラムにnil値を持つレコードは返されません。

ということだった。

そのため、このような場合は

`where(column_name: [nil, 1])`というように、OR条件で検索をかけてやる必要があった。

### 今日作成したレコードを取得
```ruby
Model.where(created_at: Time.zone.today.beginning_of_day..Time.zone.today.end_of_day)
```

### migrationファイルのtypoにご注意
`add_column`でbooleanのカラムを追加した際に、`dafault: false`を入力したつもりが`defalt: false`になっていた。

エラーも出ずにマイグレーションできちゃったので、全然気づかず、あとから「default値設定したのに入ってないやんけ。」となり、気づいた。

ご注意を。

### LIKE検索
```ruby
User.where('email LINE ?', 'user-hoge-hoge%')
```
=> emailが`user-hoge-hoge`で始まるUserを検索。


emailが`user-hoge-hoge-1_1@example.com`とか、`user-hoge-hoge-1_4@example.com`みたいなUserを検索したいときに、

```ruby
User.where('email LINE ?', 'user-hoge-hoge-1_%')
```

のようにしたら、`user-hoge-hoge-10_1@example.com`みたいな、emailもヒットした。MySQLの仕様？`_`は任意の文字列にヒットしてしまう?

