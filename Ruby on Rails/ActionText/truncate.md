## 長い文字列の一部を表示　truncateメソッドをActionTextで！

記事一覧画面などで、記事等をカード形式で表示しているときなどに、文量の多い記事が大きく表示されてしまって嫌だったので、truncateメソッドというメソッドを使って50文字以上は<...>と表示する、というような
ことをしていた。

この度、ActionTextを導入してみて、rech_textareaを使うためにはcontent(もともと使っていた記事本文のカラム)ではなく、別にbody等のカラムを用意しないといけなくなった。

（詳しくはRailsガイドのActionTextの章を参照）

それで、各所に散りばめられた`content`を`body`に修正したんだけど、急に`truncate`が`noMethodError`になった。

なんでだろうって調べていたら、[こちら](https://qiita.com/tegnike/items/999d3dc439156a1b0342)の記事に出会った。

ActionTextのrech_textareaから生まれたテキスト等はhtmlタグなどの飾りがついていたり、画像が埋め込まれている影響でtruncateが使えなくなっているのかな。

それで以下の用に、<thmlタグを外して><文字列に変換して><改行を抜いて><空文字を抜いて><`truncate`>することで、期待した動作が得られる。

```
= strip_tags(article.body.to_s).gsub(/[\n]/,"").strip.truncate(50, omission: '...<続きは下のボタンから>') 
```
