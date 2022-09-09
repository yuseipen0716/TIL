## memo

### ぱっと見わかりにくい記法たち
#### %記法
railsのコードを読んでいて、`%i[foo bar]`みたいに書かれている部分を見かけた。
(controllerのbefore_actionのところとか。)

こちらは中身をシンボルの形の配列に変換する%記法だった。
```ruby
irb(main):001:0> %i[foo baa]
=> [:foo, :baa]
```
となる。中身にはカンマいらない。注意。

%記法は他にもいろいろあるので、また追記していく。

#### \* から始まる引数
```ruby
def hoge(*bar)
  xxx
end
```
こんなようなメソッドがあったとする。引数の最初についてる`*`何？ってなったけど、調べてみたら、`*`から始まる引数は式展開をして
メソッドに渡されるらしい。
```ruby
fluit = %w[apple banana grape]
def call(a, b, c)
  puts a
  puts b
  puts c
end

call(*fluit)
=>
apple
banana
grape
```
