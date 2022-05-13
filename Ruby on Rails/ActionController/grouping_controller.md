## コントローラをグループ化

コントローラのファイルがadminとかuserとかいろいろなディレクトリに配置され、グループ化されているのを見かけた。

自分で何かを作っているときに、そのような操作をしたことがなかったので、調べてみた。


### ジェネレータコマンドで名前空間付きのコントローラを作成

```
$ rails g controller admin::users

→create  app/controllers/admin/users_controller.rb
```

### ルーティング 名前空間を追加

ディレクトリの中にusersコントローラを格納できた。あとは、名前空間に入ったコントローラを参照できるようにルーティングを書き直さないといけない。

大体のことはRailsガイドに書いてあった。
([2-6 コントローラの名前空間とルーティング](https://railsguides.jp/routing.html#%E3%82%B3%E3%83%B3%E3%83%88%E3%83%AD%E3%83%BC%E3%83%A9%E3%81%AE%E5%90%8D%E5%89%8D%E7%A9%BA%E9%96%93%E3%81%A8%E3%83%AB%E3%83%BC%E3%83%86%E3%82%A3%E3%83%B3%E3%82%B0))

``` ruby
# config/routes.rb

Rails.application.routes.draw do
  namespace :admin do
    resources :users
  end
end
```

こんな感じにしてあげればOK