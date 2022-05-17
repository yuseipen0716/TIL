## rakeタスクについてメモ

### taskの作り方

```
$ bundle exec rails g task sample
      create  lib/tasks/sample.rake
```

上記のジェネレータコマンドで新しいタスクを作成することができる。

``` ruby
namespace :sample do
    desc "<タスクの簡単な説明をここに書く> say hello"
    task :say_hello do
      puts "hello world"
    end
end
```

↑を保存し、`rake -T`と叩くと、rails コマンドが一覧表示される。たぶん `rake sample:say_hello`みたいに表示されて、
その横に、先ほど書いたdescriptionの内容が記載されているはず。

こちらが確認できたら

`rake sample:say_hello`と叩けば`hello world`が表示されるはず。

### :environment ???

rakeタスクの中でDB内に保存されている情報などを使用する場合はひと工夫必要。 

taskファイルのnamespace部分、上記のtaskファイルでいるところの3行目、`task :say_hello do`の部分に`:environment`を追記して、
`task say_hello: :environment do`という風に記載する。

この`:environment`の正体については、Railsのソースを読んで解説してくださっていたので参考にさせていただきます。

> 参考: [Rails における rake タスクの :environment について](https://qiita.com/FumiyaShibusawa/items/11035fc640bb36a615ad)


