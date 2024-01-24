## eachメソッドについてのメモ

`each_with_index`メソッドを使用すれば、配列のindexも変数として受け取ることができる。

`each.with_index(3)`のように`with_index`メソッドを使用すると、指定したindex番号から繰り返しを始めることもできる。

## 4.times.eachみたいなループでもインデックスを1から始めたい
```ruby
1.upto(4).each do |i|
p i
end
```
これでいけた。
