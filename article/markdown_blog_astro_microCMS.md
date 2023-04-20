## はじめに
前回の記事の続きです。

Astro製のブログのコンテンツ管理をmicroCMSで行っている場合に、markdown形式で記事を入稿するためにやったことの記録です。

前回はmicroCMS側の準備について書きました。今回はクライアントサイド、つまりAstro側の準備になります。

やることとしては大きく2つです。これらを行うのに必要だったライブラリや参考にさせていただいた記事も載せておきます！ 

- markdown形式で書かれたプレーンテキストをHTML形式のテキストにする
- コードブロックなどのシンタックスハイライトを設定する

## 参考にさせていただいた記事など
- [microCMSを使ってマークダウンで入力するブログを作る](https://blog.microcms.io/create_markdown_blog/)
- [サーバサイドでシンタックスハイライトを行う](https://blog.microcms.io/syntax-highlighting-on-server-side/)

## 使用したライブラリ
- [markedjs](https://marked.js.org/)
- [highlight.js](https://highlightjs.org/)
- [cheerio](https://github.com/cheeriojs/cheerio)

## markdown形式のテキストをHTMLに変換する
### markedでmarkdown形式のテキストをHTMLに変換
markdown形式のテキストをHTMLに変換するにはmarkedjsを使用しました。

markedjsを使用すれば

```
---
import Layout from "../../layouts/Layout.astro";
import { getBlogs, getBlogDetail } from "../../library/microcms";
import { marked } from "marked";

// 詳細記事ページの全パスを取得
export async function getStaticPaths() {
  const response = await getBlogs({ fields: ["id"] });
  return response.contents.map((content: any) => ({
    params: {
      blogId: content.id
    }
  }));
}
// 記事の詳細情報を取得
const { blogId } = Astro.params;
const blog = await getBlogDetail(blogId as string);

// microCMSから返されたプレーンテキストをmarkdownにparse
const html = marked.parse(blog.content, { gfm: true })
---

<Layout title="My first blog with Astro">
  <main>
    <h1 class="title">{blog.title}</h1>
    <p class="publishedAt">公開日時 : {blog.publishedAt}</p>
    <div class="post" set:html={blog.content}></div>
    <div class="post" set:html={html}></div>
  </main>
</Layout>
```

という具合でmarkdown形式のテキストをparseし、HTML形式に変換した後、`<div class="post" set:html={html}></div>`という形でtemplateとして出力しています。

`const html = marked.parse(blog.content, { gfm: true })`というように、`gfm: true`をoptionとしてparseすることで、GFM(GitHub Flavored Markdown)として変換してくれます。

これで一旦、markdown形式で入稿した記事をブログ記事ページで表示できたのですが、すこし気になることがありました。

この状態だと、シンタックスハイライトがあたっていなかったり、コードフェンスで囲った部分の背景色が変わっていないなどの問題がありました。

調べるとhighlight.jsというパッケージがあるようでした。こちらを使ってシンタックスハイライトを当てるなどの実装を行います。

### highlight.jsを使用してシンタックスハイライトをあてる
markedjsを使用してHTMLにした後でhighlight.jsを利用してシンタックスハイライトを当てる方法は検索すれば多く出てきますが、今回のようなJamstackの構成では動作がおかしな部分がありました。

初回アクセス時にはシンタックスハイライトや、コードブロックの背景色などのスタイルが当たっておらず、一度リロードすると表示されるという具合の動作になってしまっておりました。

これは、AstroがSSRを採用しており、クライアントサイドでシンタックスハイライトを当てるような処理が実行されず、初回アクセス時に上記のようなスタイルが当たっていない状態に陥っていたと思われます。

そのため、参考記事にも挙げさせていただいた、[サーバサイドでシンタックスハイライトを行う](https://blog.microcms.io/syntax-highlighting-on-server-side/) の記事を読んで、サーバーサイドでのシンタックスハイライトを行う
ように修正しました。

以下がその手順です。

#### 1. src配下にutilsディレクトリを作成し、markdownHighlight.tsを作成
```
mkdir src/utils
```

[cheerio](https://github.com/cheeriojs/cheerio) というライブラリを使用します。こちらにhtml形式のテキストを渡すと、jQueryのような操作でclass名を付与したり、styleを当てることができました。


```
// src/utils/markdownHighlight.ts

import cheerio from "cheerio";
import hljs from "highlight.js";

export function highlightMarkdown(html: string) {
  const $ = cheerio.load(html);
  // codeブロックの部分のsyntax highlightを行う。
  // 初回アクセス時にもスタイルが当たっているように。
  $('pre code').each((_, elm) => {
    const result = hljs.highlightAuto($(elm).text());
    $(elm).html(result.value);
    $(elm).addClass('hljs');
  });

  // p code部分にも網掛けのスタイルを当てたい
  //（当たっているが、サイトの背景色の関係で見えないため。網掛けを濃く）
  $('p code').each((_, elm) => {
    $(elm).html();
    $(elm).addClass('hljs');
  });

  return $.html();
}
```

#### 2. ブログ記事のmarkdown -> HTMLの処理を行っている部分の修正
```
---
// src/pages/articles/[blogId].astro
import Layout from "../../layouts/Layout.astro"
import { getBlogs, getBlogDetail } from "../../library/microcms";
import { highlightMarkdown } from "../../utils/highlightMarkdown";
import { marked } from "marked";
// codeのsyntax highlightのthemeを変えたいときは以下を変更(demo: https://highlightjs.org/static/demo/)
import 'highlight.js/styles/vs2015.css'

// 詳細記事ページの全パスを取得
export async function getStaticPaths() {
  const response = await getBlogs({ fields: ["id"] });
  return response.contents.map((content: any) => ({
    params: {
      blogId: content.id
    }
  }));
}

// 記事の詳細情報を取得
const { blogId } = Astro.params;
const blog = await getBlogDetail(blogId as string);

// microCMSから返されたプレーンテキストをmarkdownにparse
const parsed = marked.parse(blog.content, { gfm: true })
// 最終的にtemplate部分で表示するhtml
const html = highlightMarkdown(parsed)
---

<Layout title="My first blog with Astro">
  <main>
    <h1 class="title">{blog.title}</h1>
    <p class="publishedAt">公開日時 : {blog.publishedAt}</p>
    <div class="post" set:html={blog.content}></div>
    <div class="post" set:html={html}></div>
  </main>
</Layout>
```

このようにすることで、サーバーサイドでのシンタックスハイライトを実装でき、初回アクセス時にもコードブロックなどのスタイルがきちんと当たっている状態になりました。

シンタックスハイライトやコードブロックの背景色以外にも、h2タグには下線を引きたいなど、当てたいスタイルがあれば`markdownHighlight.ts`に変換時の処理を追加することで実現できます。

こちらのブログサイトのソースコードはGitHub上で公開していますので、気になった方はhttps://github.com/yuseipen0716/yusei_note こちらへどうぞ。

