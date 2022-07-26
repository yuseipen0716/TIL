## hamlで後置ifを書いてクラスの出しわけ

``` ruby
%li.navbar-default
```

のようなクラスを書いているときに、条件によって`.navbar-default`を出し分けたい場合に後置ifを使おうと思った。

hamlで後置ifを書くのは初めて、というかできるかわからなかったけど、一応やりかたはあるようだった。

``` ruby
%li{ class: "#{'navbar-default' if foo}" }
```

みたいに書いたらできました。

正直、可読性は下がると思うのでご利用は計画的に。あと、ちゃんとコメント入れた方がいいかも


[【Rails6, haml】クラス名にindexをつける方法【each_with_indexまたはeach.with_index】](https://qiita.com/pharma_tech3/items/1d9bf30432f639f35152)

こちらもhamlでクラス名を書くときに役に立つときがあった
