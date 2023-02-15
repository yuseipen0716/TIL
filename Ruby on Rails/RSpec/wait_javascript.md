## JavaScriptの描画等を待つ

RSpecのsystem_specでボタンなどの要素を指定したが見つからない、とテストが落ちるときがある。

JavaScriptのコードは間違っていないが、それが表示される前に要素の検索が行われ、要素を見つけられないようだ。

自分がかかわったprojectではsleepメソッドで処理を待機させているコードを見かけたことがあるが、あまりよくない？ようだ。

EverydayRailsを読んでいたら、Capybaraの`using_wait_time`メソッドについて書かれていた。

Capybaraはデフォルトでは要素検索の際の待ち時間は2秒らしい。2秒経っても要素が見つからない場合はテスト失敗してしまう。

Capybaraの設定をしている部分で

```ruby
Capybara.default_max_wait_time = 10
```

とすると、デフォルトの待ち時間が10秒になる。

全てのコードにこの設定を適用したくない場合などは以下のように`using_wait_time`メソッドを使用するのがよさそう。

```ruby
scenario :runs a really slow process" do
  using_wait_time(10) do
    # test_code
    # 10秒経っても目的の要素が見つからない場合はテスト失敗
  end
end
```

> いずれにしても基本的なルールとして、処理の完了を待つためにRubyのsleepメソッドを使うのは避けてください

とのこと。これを意識して、コードのリファクタリングをしていこう。



### 参考
[Everyday Rails - RSpecによるRailsテスト入門](https://leanpub.com/everydayrailsrspec-jp)
[Capybara doc using_wait_time](https://www.rubydoc.info/gems/capybara/Capybara.using_wait_time)
