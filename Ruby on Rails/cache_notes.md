## キャッシュについてのメモ

今回やっていた実装で、キャッシュに関してかなりなやまされたので復習する。

### キャッシュのログを見る方法
- `rails dev:cache`で開発環境でもキャッシュを使うように設定
- `config/environments/development.rb`内の`config.log.level = :info`をコメントアウトする → サーバのログにSQLのログなどが表示される
- `config/initializers/cache.rb`を作成 → `ActiveSupport::Cache::Store.logger = Rails.logger`を記載。 →Rails.cacheの実行内容がログに吐かれる

### フラグメントキャッシュ
Railsに組み込みで入っているキャッシュ機能。

### 低レベルキャッシュ
