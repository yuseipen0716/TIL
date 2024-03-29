#マークダウン記法自分用メモ
---

たまに仕様が変わる？ため、最新版は[こちら](https://docs.github.com/ja/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax) のドキュメントを確認した方がよさそう。

マークダウン記法に慣れていきたい。

## 目次

- [そもそもマークダウン記法ってなに](https://github.com/yuseipen0716/TIL/blob/main/markdown_memo.md#:~:text=%E3%82%B9%E3%83%A9%E3%83%83%E3%82%B7%E3%83%A5%E3%81%A7%E3%82%A8%E3%82%B9%E3%82%B1%E3%83%BC%E3%83%97-,%E3%81%9D%E3%82%82%E3%81%9D%E3%82%82%E3%83%9E%E3%83%BC%E3%82%AF%E3%83%80%E3%82%A6%E3%83%B3%E8%A8%98%E6%B3%95%E3%81%A3%E3%81%A6%E3%81%AA%E3%81%AB,-%E6%96%87%E6%9B%B8%E3%82%92%E8%A8%98%E8%BF%B0)
- 見出し
- 改行
- コード
- 引用
- リスト
- 水平線
- リンク
- テーブル
- バックスラッシュでエスケープ
- 打消し線
- 
---

## そもそもマークダウン記法ってなに 

- 文書を記述するための軽量マークアップ言語のひとつ
- 書きやすくて読みやすいプレーンテキスト形式の文書をHTMLに変換して見た目を整えてくれる（ちなみにプレーンテキストっていうのは、コンピュータ上で文章を扱うための一般的なファイルフォーマットまたは、文字列の形式のこと）
- 使うサービスや環境によって機能が拡張されていたりするため、同じ書き方をしてもプレビューで若干の差異がでることがあるみたい。
- ジョン・グルーバーさんっていう人が開発してくださったそうな。ありがとうございます。Special thanks for development of the Markdown. 

## 見出し

見出し文字にしたい時は `#`を行頭につける。  `#`をつける数でhtmlのh1〜h6まで対応

# 見出し1
## 見出し2
### 見出し3
#### 見出し4
##### 見出し5
###### 見出し6

```
# 見出し1
## 見出し2
### 見出し3
#### 見出し4
##### 見出し5
###### 見出し6
```
見出し1と見出し2には勝手にアンダーラインも入れてくれるんだね。


## 改行
改行したい！時は末尾にスペースをふたつ  
`改行！　　`

一行あけて改行したいときは

そのまま改行して一行開けて書き始めればOK
```
改行！！

改行できた！！！
```


## コード
```
`（バッククォート）
```
で囲うと`インラインコード`に！
```
```（バッククォート3つ）
```
で囲うとブロック要素のコードに！

````
コードブロックの中にescapeしたコードブロックを書きたい
```
inner codeblock
```

\```
スラッシュエスケープを試す
\```

-> できなかった。
一番外側のバッククオートを一つ増やして````したら行けた。

````

```
```rubyみたいに、最初に書く```のあとにrubyとかphpみたいに言語名を入れてあげると色付きのコードになる
```

```ruby

# 21の倍数かどうか　if文を練習
number = 47
puts number
if number % 21 == 0
  puts "21の倍数です"
elsif number % 7 == 0
  puts "7の倍数です"
elsif number % 3 == 0
  puts "3の倍数です"
else number % 7 != 0 || number % 3 != 0
  puts "3の倍数でも7の倍数でもありません"
end

```


## 引用
` > `を行頭につけると引用の記述ができる

> こんな感じ

```
> 複数行引用する
> 時はこんな感じに
> 全ての行頭に > をつけます
```
> 複数行引用する  
> 時はこんな感じに  
> 全ての行頭に  > をつけます  
↑ちゃんと改行しないと一行の引用になっちゃうからね。

```
>> こんな風に　>　をふたつつければ多重引用？ができるみたいだけど　  
>> どこで使うんやろうか  
```
>> こんな風に　>　をふたつつければ多重引用？ができるみたいだけど　  
>> どこで使うんやろうか  

## リスト
```
- list1
- list2
- list3
  - n.list1
  - n.list2
- list4
```
- list1
- list2
- list3
  - n.list1
  - n.list2
- list4

` - `と半角スペースのあとに文字を入れると、箇条書きのリストになる

` 1. `の後に文字を入れると番号付きリストになる  1.2.3....と番号を順番に並べなくてもいい。全部` 1. `でよし

```
1. list1
1. list2
1. list3
    1. n.list1
    1. n.list2
1. list4
```
1. list1
1. list2
1. list3
    1. n.list1
    1. n.list2
1. list4

ulリストもolリストもネストを作りたいときにはtabを押して行頭の位置を後ろにもっていく。

olの方はtab2回押さないとだめだった！

## 水平線
```
太い
***
太い
___
細い
---
```
` * `とか` _ `とか` - `を３つ以上、水平線を引きたい文字列の下の行に入力

太い
***
太い
___
細い
---

## リンク
```
[Twitter](https://twitter.com)
```

[Twitter](https://twitter.com)

```
[Twitter]: https://twitter.com
[Twitter]
```

こんな風に、`[Twitter]`を変数みたいに扱って、呼び出す書き方もできる。  こっちの方が使い所多そうだね。

[Twitter]: https://twitter.com
[Twitter]

同一プロジェクト内のドキュメントに対するリンクは相対パスで指定できる

[Docker下でRails、Next.jsの環境をつくる](./Docker/RailsNext.md)

```
[Docker下でRails、Next.jsの環境をつくる](./Docker/RailsNext.md)
```


## テーブル

```
こんな感じ|に  
-------|----
すると   |テーブル  
が      | 作れます
```

こんな感じ|に  
-------|----
すると   |テーブル  
が      | 作れます

うまく作れない時もあるのだけど、やり方が間違っているのだろうか

## バックスラッシュでエスケープ

\*や｀などの特殊文字を記載したいが、普通に書くとマークダウン記法が適用されてしまう

```
\* \# こんなふうに\を使用してエスケープする
```

```
\`ショートコード\`
```

\`ショートコード\`

## 打消し線
```
~~打消し線~~
```

~~打消し線~~

## チェックボックス

- [ ] チェックボックスも作れる(`- [ ] チェックボックスも作れる`)
- [x] チェックを入れる(`- [x] チェックを入れる`)


## アラート構文

> [!NOTE]
> `> **Note**`

> [!TIP]
> `[!TIP]`

> [!IMPORTANT]
> `> [!IMPORTANT]`

> [!WARNING]
> `> [!WARNING]`

> [!CAUTION]
> `> [!CAUTION]`




