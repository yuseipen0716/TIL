## デバッグについてのいろいろ

###  config.log_level

logを出す度合いを決めている設定。config/environment/development.rbの`config.log_level = :info`の記述をコメントアウトするか、info -> debugに変更すると、SQLなどのクエリのログ
もみられる。

---

### 現在実行中のcontrollerとaction名を出力する
以下をapplication_controllerに記載。application_controllerが複数のnamespaceに存在する場合は、対象のnamespaceのapplication_controllerに記載しないと、出力されないので注意。

``` ruby
before_action :output_current_action
def output_current_action
  p "#{controller_path}##{action_name}"
end
```

### メソッド名の出力
ループ処理の中などで、どのタイミングで、どのメソッドが呼ばれているのか迷子になった場合などを知りたいときに。

```ruby
def hoge
  p "💡 Called: #{__method__}"
  # hogeメソッドの処理がつづく
end
```

のように、メソッド名を出力するための__method__を書くメソッドの頭に差し込む
