## Rails scaffoldについて

Railsでアプリケーションを作成する際に、scaffoldというコマンドを使用しているのをみたけど、あまりよく知らなかったので調べた。

CRUD機能を実装してくれるみたい。


### scaffoldって何

- Railsに備わっているコマンドの一つ

- ルーティングやMVC、テーブルの記述やファイルを自動生成してくれる

- index, show, new, edit, create, update, destroyの7つのルーティングが作成される。

### 使い方

` $ rails g scaffold モデル名 カラム名:データ型 カラム名:データ型 ・・・ `

マイグレーションファイルが作成される。

` $ rails db:migrate `


