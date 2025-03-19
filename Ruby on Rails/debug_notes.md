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

これでもよさそう

```ruby
before_action -> { p "#{controller_path}##{action_name}" }
```

### メソッド名の出力
ループ処理の中などで、どのタイミングで、どのメソッドが呼ばれているのか迷子になった場合などを知りたいときに。

```ruby
def hoge
  p "💡 Called: #{__method__}"
  # hogeメソッドの処理がつづく
end
```

のように、メソッド名を出力するための`__method__`を各メソッドの頭に差し込む

---

### callerの出力
何か問題のある処理があるとして、その処理が呼び出される前はどのようなメソッド呼び出しがあったのかを負いたい場合、callerを出力することでTraceを確認できる。

Traceを確認したい部分に以下を埋め込み、当該処理を動かしてみる。

```ruby
puts "Caller Trace:\n" + caller.select { |c| !(c.include?('vendor') || c.include?('bundle')) }.join("\n")
```

上記の簡単な説明としては、

- 現在の処理までの処理呼び出しの流れのTraceを表示する
- gemなどが関連する呼び出しに関しては出力しないように、pathに`vendor`や`bundle`が含まれるものは弾いている
  - getなどが関連する部分の出力を見たいのであれば、select以降は消してね。

putsだと、pumaなどのログには出力されるが、development.logなどには記載されないと思うので、その場合は、`puts`を`Rails.logger.debug`などに置き換える。

普通に`raise`を差し込むだけでか行けるするような気もするが。
