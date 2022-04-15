## Service Workerについてまとめ

アプリを閉じている（ブラウザを開いていないとき)でもpush通知が飛ばせるようにする必要があり、調べていたらService Worker、Push APIというものを知った。

ドキュメントを読んでいたら、単純なJavaScriptの話だけではなく、さまざまなWebを支える技術のようなものの理解が必要なようで初見ではちんぷんかんぷんだったので、噛み砕いて理解できるようにまとめてみる。

まとめる前に、今やりたい、欲しい機能

- 時間計測開始後○時間経過した時点での通知
- タスクの期限が近づいているときのリマインド
- Webブラウザを落としているときでも通知が来るように。
- 個別ユーザに向けての通知
- スマホユーザーにも通知を飛ばせるように

さっとみてみた感じ上記のService WorkerとPush APIを使用すればできそうな気もするが…

下記リンクの「Service Workerの使用」のリンクより、事前準備や実装の流れについて学べます。






### 参考

- [Service Worker API](https://developer.mozilla.org/ja/docs/Web/API/Service_Worker_API)
- [Service Workerの使用](https://developer.mozilla.org/ja/docs/Web/API/Service_Worker_API/Using_Service_Workers)
- [Push API](https://developer.mozilla.org/ja/docs/Web/API/Push_API)
- [Web Workers API](https://developer.mozilla.org/ja/docs/Web/API/Web_Workers_API)
- [[PWA]プッシュ通知を実現するPushAPIとNotificationAPI](https://qiita.com/ozaki25/items/b35e5c907c756e704d23)
- [ネイティブアプリ風Webアプリ「PWA」を実現する3つの技術](https://knowledge.sakura.ad.jp/23201/)
- [サーバーからブラウザを通じてデスクトップ通知する方法（Push API を利用）](https://laboradian.com/web-push/)
- O'REILLY 第7版 JavaScript
- [Web Fundamentals(ウェブアプリへのプッシュ通知の追加)](https://developers.google.com/web/fundamentals/codelabs/push-notifications)
- [意外とカンタン！流行のWebプッシュ通知はJavaScriptでサクッと実装できる](https://www.webprofessional.jp/browser-notification-api/)
