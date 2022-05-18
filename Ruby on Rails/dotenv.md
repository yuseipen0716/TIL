## .envファイルで安全にKeyを管理

APIキーなどをソースにべた書きしてバージョン管理システムにあげるのは危険。

それらは環境変数化しておく方が安全

dotenv-railsっていうgemがあったので、それを使ってみる。

> 参考: [【Rails】 初心者向け！gem 'dotenv-rails'の使い方](https://qiita.com/__Wata16__/items/d2d3aa21623f8c026d5d)

