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

