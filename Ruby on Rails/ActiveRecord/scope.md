## scopeについて

モデルにScopeと呼ばれるものを定義することができる。Scopeというのは、よく使われる検索条件に名前をつけてひとまとめにしたもの。

- 繰り返し利用するクエリの再利用性向上
- クエリに名前がついているため、可読性があがる

という利点がある。

``` Ruby
(users/modelファイル)
class User < ApplicationRecord
  scope :tall, -> {where("tall > ?", 175)}
end
```
みたいな感じ

`User.tall`で呼び出せる。

## クラスメソッドで検索条件を定義した時の動作の違い

