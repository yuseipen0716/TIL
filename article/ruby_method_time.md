## はじめに
Rubyで大変重い処理を書いてしまい、ボトルネックがどこなのか等調査したくなり、実行時間を吐かせるようにしたので、その時の備忘録。

## 環境
Ruby 3.1.0
Rails 7.0

## 方法
以下のように、処理開始の時刻を変数に入れ、時間を調べたい所でその時点の時刻との差を出力するようにした。

```ruby
def kuso_omoi_syori
  # 開始時間
  start_time = Time.zone.now

  # いろんな処理
  # ...

  puts "=============================================="
  puts ""
  puts ""
  puts ""
  print "\e[31m"
  puts "     実行時間: #{Time.zone.now - start_time}秒     "
  print "\e[0m"
  puts ""
  puts ""
  puts ""
  puts "=============================================="

  # 処理の最終結果の式
  # ...
end
```

上記の処理を動かすと

![image](https://user-images.githubusercontent.com/81737622/235419229-595e5e29-4358-48a6-86bf-9742f097819a.png)

のようにログが出力される。

## 参考
[Rubyで出力に色を付ける方法](https://qiita.com/khotta/items/9233a9ffeae68b58d84f)
