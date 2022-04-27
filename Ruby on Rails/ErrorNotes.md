## Rails エラーメモ

### ActionController::UnknownFormat

フォームにわざとバリデーションエラーが出るように入力してsubmitすると、こんなエラーが。
大概このエラーって遷移先のテンプレートがないよって場合が多いが、今回はそちらは用意済み。
```
ActionController::UnknownFormat in TicketsController#create
TicketsController#create is missing a template for this request format and variant.
```
ログ
```
Started POST "/events/13/tickets" for ::1 at 2022-02-25 02:30:35 +0900
Processing by TicketsController#create as HTML
  Parameters: {"authenticity_token"=>"[FILTERED]", "ticket"=>{"comment"=>"参加します参加します参加します参加します参加します参加します参加します"}, "button"=>"", "event_id"=>"13"}
  User Load (0.1ms)  SELECT "users".* FROM "users" WHERE "users"."id" = ? LIMIT ?  [["id", 1], ["LIMIT", 1]]
  ↳ app/controllers/application_controller.rb:13:in `current_user'
  Event Load (0.0ms)  SELECT "events".* FROM "events" WHERE "events"."id" = ? LIMIT ?  [["id", 13], ["LIMIT", 1]]
  ↳ app/controllers/tickets_controller.rb:7:in `create'
Completed 406 Not Acceptable in 3ms (ActiveRecord: 0.1ms | Allocations: 3343)


  
ActionController::UnknownFormat (TicketsController#create is missing a template for this request format and variant.

request.formats: ["text/html"]
request.variant: []):
```

saveメソッドをsave!に切り替えてバリデーションエラーの例外を出してあげるとこんな感じ
```
ActiveRecord::RecordInvalid in TicketsController#create
バリデーションに失敗しました: コメントは30文字以内で入力してください
```

バリデーションエラーがでちゃうからsaveできなくて、エラーが出ている感じだけど、本当はerbで書いたdocument.getElementById("createTicketErrors").innerHTML = "<%= j render("errors", errors: @ticket.errors) %>"`これでバリデーションエラーを拾って、画面上に表示したい。

##### 解決策 -> Ajaxをoffに
` local: false`　を入力フォームの部分に。こうしたらできたが、、、Ajaxとかその辺りについて、要勉強です

---

### factory_botレコード追加時の Factory not registered:エラー

factory_botを導入してモデルを作成。rails consoleでレコード追加のお試しをしようと`FactoryBot.create(:event)`を入力したら`Factory not registered: `のエラーがでた。

調べてみると、spring stopを実行したり、spec_helper.rbの記述内容を変更したりする対応策が色々と紹介されていた。自分の場合はそれらを行なってもうまく行かず、factory_botのgemをインストールし直したら治った。

rails consoleも再起動した。

### [Windows]tzinfo-data is not present. Please add gem 'tzinfo-data' to your Gemfile and run bundle install (TZInfo::DataSourceNotFound)

`rails s`や`rails g`などのコマンドを打つと上記のようなエラーが出る。Mac使っていた時はでなかったのでWindows固有のものでしょうか。

調べてみると、railsプロジェクトを立ち上げた際にGemfileに記載される以下の行が原因らしかった。

``` ruby 
gem 'tzinfo-data', platforms: [:mingw, :mswin, :x64_mingw, :jruby]
```

[こちら](https://qiita.com/tatama/items/3f0f5e42cb5f75b53817)に4つの解決策が載っていた。

1番上の「`gem 'tzinfo-data', platforms: [:mingw, :mswin, :x64_mingw, :jruby]`の`, platforms: [:mingw, :mswin, :x64_mingw, :jruby]`部分を削除して`bundle update`するというもの。

これでとりあえず解決。







