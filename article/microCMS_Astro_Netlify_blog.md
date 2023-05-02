## はじめに
当ブログサイトを作成した際の手順などを書き残しておくものです。

## 参考にさせていただいた記事等
- [[Qiita]microCMS & Astro で永久0円のJamstack爆速ブログを作成する丁寧なハンズオン](https://qiita.com/Michinosuke/items/09c30fde4ca7168f96ef)
- [AstroとmicroCMSでつくるブログサイト](https://blog.microcms.io/astro-microcms-introduction/)
- [Astroドキュメント](https://docs.astro.build/ja/getting-started/)

## 利用しているサービス等について
今回のブログサイト作成では、NetlifyというWebホスティングサービスを利用し、またコンテンツの管理にはmicroCMSを利用しました。

### Webホスティングサービス
Webホスティングサービスには他に[Vercel](https://vercel.com/)や[Firebaseホスティング](https://firebase.google.com/docs/hosting?hl=ja)などがありますが、今回は自分が以前他のプロジェクトで使用していて使い方も多少心得があり、GitHubとの連携も容易だと感じたNetlifyを利用しました。

### ブログのコンテンツ管理
ブログのコンテンツ作成に関しては、直接markdownファイルを作成してGitHubのリポジトリにpushする方法でもいいかと思っていたのですが、上記の参考にさせていただいたJamstackのハンズオンにてmicroCMSに触れていたこともあり、microCMSを利用することにしました。ブログの入稿に関して、やや躓く部分もあったので、別でブログ記事を作成出来たらと思います。

### Webフレームワーク
今回、ブログ作成に使用したWebフレームワークはタイトルにもある通り、Astroです。

Astroを選ぶ理由というページが[公式ドキュメント](https://docs.astro.build/ja/concepts/why-astro/)にも書かれており、可能な限りSSRを利用するようにしているAstroではサイトの動作が高速ということでした。ページの表示の遅さは結構なストレスになりますから、できることなら速いサイトを作ってみたいという気持ちがありました。

また、Astroは極端な話、HTMLとCSSがなんとなく分かれば、利用できるくらい構文がわかりやすいのもいい点かと思います。JSXで書いたコンポーネントをアプリ内で利用することも可能なようで、非常に柔軟性が高いと感じました。

公式のドキュメントも非常にわかりやすく、まだ一部は英語ドキュメントですが、ほとんど日本語訳もされており読みやすいと思います。また、今では翻訳して読んだり、話題のchatGPTを利用するなどすれば、英語のドキュメントもそれほど難易度の高いものではなくなったかもしれませんね。

## やっていく
上記にある利用するサービスのアカウント登録などは済ませておきます。

GitHubのリポジトリも作成しておきましょう。README.mdはAstroプロジェクト作成時に合わせて作られるので、個々では不要かと思います。

1. 自分はmicroCMSのアカウント登録 & Blog APIの作成
2. Astroプロジェクトの作成
3. 作成したAstroプロジェクトに、1で作成したAPIの環境変数やmicroCMSとの連携
4. Netlifyでプロジェクト作成 → GitHubとの連携
5. GitHubで作成したリポジトリにアプリケーションの変更をpushした際や、microCMSで記事を作成した際に、ホスティングされているブログサイトがきちんと更新されるか、確認

という感じの手順で進めました。

### Astroプロジェクト作成とmicroCMSの連携
基本的には、microCMSさんの公式ブログにある「[AstroとmicroCMSでつくるブログサイト](https://blog.microcms.io/astro-microcms-introduction/)」の記事が大変詳しく書かれていたため、詳細の手順については割愛します。少しだけ補足したい所を以下に書き記しておきます。

Astroプロジェクトを作成（`yarn create astro`）すると以下のようなインストールウィザード（create-Astroというらしい）が立ち上がります。プロジェクトのディレクトリのパスや、使用するtemplateなどを聞かれます。これらはastroのバージョンにより、画面の表示やメッセージなどが変わる恐れがあるので、参考にさせていただいたサイトや今回の補足内容と表示が異なる恐れがあります。[Astroを自動CLIでインストール（公式ドキュメント）](https://docs.astro.build/ja/install/auto/)を参考にするのが最も確実化と思いますので、迷ったらこちらへ。

![image](https://user-images.githubusercontent.com/81737622/230755270-c2455c6d-2153-4eac-b8d2-a04f8a22e998.png)

自分は「どのtemplateを使ったらいいんだろう？」と迷ったのですが、一番simpleな、ウィザードで(recommended)の表示があるものでいいかと思います。

あとはdependenciesをinstallするかどうか、typescriptを使用するかなどを聞かれましたが、これはお好みでどうぞ。

GitHubのリポジトリに関する質問だけは、今回は事前にリポジトリを作成していたため「No」を選択しました。

ウィザードによるAstroインストールが終了したら、プロジェクトのディレクトリに移り、`yarn start`するとAstroプロジェクトが立ち上がり、以下のような画面になりました。

![image](https://user-images.githubusercontent.com/81737622/230757567-fd9cea15-5321-45ac-91df-6e488b69cb9e.png)

ここまでできたら、あとはブログ記事を今立ち上がったプロジェクトで表示するだけです。

その詳しい方法については先ほど記載したmicroCMSさんの公式ブログの記事がわかりやすいかと思います。

### NetlifyとGitHubの連携 + デプロイしてみる
それでは作成したサイトをNetlifyでホスティングしてもらうために色々と設定します。

#### GitHubとの連携
Netlifyにsign inして、プロジェクトを作成したり、GitHubのリポジトリと連携させる部分については「[[Qiita]はじめてのNetlify: git pushでサイトを公開する極小手順](https://qiita.com/suin/items/743fe6252ad8af425c5e)」の記事がわかりやすいかと思います。

#### Astro上で必要な追加操作
Astroの公式ドキュメント内に、Netlifyを利用してサイトをデプロイする方法についてのドキュメント（[こちら](https://docs.astro.build/ja/guides/deploy/netlify/)）がありました。こちらのサイトはまだ未翻訳でしたが、アプリケーションに追加する必要のあるパッケージや設定などがわかりやすく書かれていました。

自分は今回、SSRを利用してみたかったため、SSR用のアダプターを導入しました。

こちらの導入も簡単な`astro add`コマンドが用意されており、こちらを実行すると

![image](https://user-images.githubusercontent.com/81737622/230756359-94a5dcd9-e076-44eb-8da1-72d5123d1bc6.png)

のようなウィザードが立ち上がり、必要な設定ファイルの記述やパッケージのインストールを行ってくれます。

上記コマンドを実行後のdiff
```
diff --git a/astro.config.mjs b/astro.config.mjs
index 882e651..52c65e4 100644
--- a/astro.config.mjs
+++ b/astro.config.mjs
@@ -1,4 +1,9 @@
 import { defineConfig } from 'astro/config';

+import netlify from "@astrojs/netlify/functions";
+
 // https://astro.build/config
-export default defineConfig({});
+export default defineConfig({
+  output: "server",
+  adapter: netlify()
+});
```

package.json
```
diff --git a/package.json b/package.json
index 999ccbd..551befd 100644
--- a/package.json
+++ b/package.json
@@ -10,6 +10,7 @@
     "astro": "astro"
   },
   "dependencies": {
+    "@astrojs/netlify": "^2.2.1",
     "astro": "^2.2.1"
   }
-}
\ No newline at end of file
+}
```

最後にプロジェクトrootに`netlify.toml`というファイルを作成します。Netlifyが連携しているリポジトリのこのtomlファイルを読み取り、自動デプロイを行ってくれるとのことです。

```
[build]
  command = "npm run build"
  publish = "dist"
```

上記の設定がすべて完了したら、GitHubのリポジトリにpushし、実際にサイトがデプロイされているかを確認してみてください。

## まとめ
以上がmicroCMS + Astro + Netlifyで作成するブログの作り方になります。

あとはAstroのドキュメントを読みながらHeaderのコンポーネントを作ってみたり、Google Analyticsを導入してみたり...と色々とサイトをいじってみましょう。

今回紹介した構成では、基本的には料金も無料で運用できるかと思います。以前WordPressを利用して、レンタルサーバを借りてブログ運用をしていた時はランニングコストが割とかかって若干の負担感がありましたが、今回のような形で無料で、なおかつ拡張性に富んだサイトを作成できるのはすごいですね...！

今後も、公式ドキュメントを読み進めながら、少しずつこちらのサイトをいい感じのものに仕上げていけたらと思います。


