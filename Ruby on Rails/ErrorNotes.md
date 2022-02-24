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
` local: false,`　を入力フォームの部分に。こうしたらできたが、、、Ajaxとかその辺りについて、要勉強です

---
