## はじめに
Astroで作成した当サイトの記事入稿についてはmicroCMSを利用しています。

リッチエディタは便利で使いやすいが、自分はmarkdown記法が好きです。

リッチエディタでもmarkdown記法は使用できるが、自分はGitHub上でmarkdownを書くことが多く、GFM（GitHub Flavored Markdown）が好きなので、同にも書き味に慣れない部分がありました。

そのため、microCMSから記事を入稿するときにmarkdown形式のテキストを入力して記事を入稿するようにしたかった。

microCMSさんのブログでもいくつか記事を読ませていただき、参考にしながらAstro製のブログにmarkdown形式のテキストを入稿し表示することができたので、その時に苦戦したことなどを書き記しておく。

一つの記事にすると長くなって、かったるくなるので2つか3つに分けます。今回はmicroCMS側の準備編です。

## 参考にさせていただいた記事など
- [microCMSを使ってマークダウンで入力するブログを作る](https://blog.microcms.io/create_markdown_blog/)
- [microCMSの拡張フィールドを利用したMarkdownの入稿環境をつくる](https://blog.microcms.io/field-extension-markdown-editor/)
- [サーバサイドでシンタックスハイライトを行う](https://blog.microcms.io/syntax-highlighting-on-server-side/)

microCMSの拡張フィールドを利用した入稿環境はとても魅力的でしたが、今回は一旦前者のプレーンテキストでmarkdown形式のテキストを書き、Astro側でHTMLにparseして表示する形をとりました。

拡張フィールドを使用する方法にも挑戦してみたいです。

## 使用したライブラリ
- [markedjs](https://marked.js.org/)
- [highlight.js](https://highlightjs.org/)
- [cheerio](https://github.com/cheeriojs/cheerio)

## microCMS準備編
これまでリッチエディタで利用していたmicroCMSのブログAPIのAPIスキーマを変更します。（これまでリッチエディタで作成したコンテンツは壊れてしまう可能性があります...）

以下の画像のようにブログの内容（本文）の部分のコンテンツの種類を「テキストエリア」に変更します。

![image](https://user-images.githubusercontent.com/81737622/230917831-6dfebd2c-d719-43d1-ba18-dd87b4ad4b7a.png)

microCMS側の準備はこれだけでOKです。

これで、リッチエディタ形式の入稿環境からプレーンテキストでの入稿環境に代わりました。

この状態でmarkdown形式でテキストを書き、記事を入稿するとAPIから取得できるcontentsは以下の画像のようになります。

![image](https://user-images.githubusercontent.com/81737622/231041959-4cd4d3c9-d53d-4baa-9bdf-68a4eaa55c42.png)

これまでのリッチエディタで入稿していた時は、この部分がHTML形式のデータになっていましたが、今はプレーンテキストになっているため、このままではクライアントサイドでうまく表示できません。

そのため、次の記事ではAPIから取得したmarkdown形式で書かれたブログ本文をHTMLにパースして表示することを書いていこうと思います。

今回は大変短いですが、このあたりで。
