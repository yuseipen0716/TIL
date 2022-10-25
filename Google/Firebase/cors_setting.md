## Storage内のファイルのダウンロードリンクを作成しようとして、こけた話

Firebase Storageの中に保存してあるファイルのダウンロードリンクを取得したいと思い、getDownloadLink()メソッドを使用したところ、以下のようなエラーが出た。

```
Access to XMLHttpRequest at 'http://localhost:3000' from origin 'http://localhost:3000' has been blocked by CORS policy:
Response to preflight request doesn't pass access control check: It does not have HTTP ok status.
```

firebaseによってhostingされているページと、storage上に存在するファイルではドメインが異なるらしく、
異なるCross Origin Resource Shareのpolicyで弾かれているようだ。

これらのエラーについて以下のサイトが参考になった。

- [Firebase StorageにCORSの設定をする](https://qiita.com/niusounds/items/383a780d46ee8551e98c)
- [なんとなく CORS がわかる...はもう終わりにする。](https://qiita.com/att55/items/2154a8aad8bf1409db2b)

やることとしては
- Google Cloud SDKをinstall
- Google Cloud SDKの初期設定
- CORSの設定を記述したJSONファイルを作成
- 作成したJSONファイルをデプロイ

Google Cloud SDK関連の操作についてはTILの[こちら](https://github.com/yuseipen0716/TIL/blob/main/Google/Firebase/storage_lifecycle.md)に大まかに記載してあるので、参考に。

CORSの設定ファイルについては、任意のディレクトリでcors.jsonという名前で作成した。

```
# cors.json
   [{

     "origin": ["https://<firebaseのURL>.web.app/", "http://localhost:3000"], // localでも試したかったので

     "responseHeader": ["*"],

     "method": ["GET"],

     "maxAgeSeconds": 86400

   }]
```

作成後、デプロイ

```
$ gsutil cors set cors.json gs://xxxx.appspot.com
```

変更が反映されたか確認
```
$ gsutil cors get gs://react-resume-sample.appspot.com
```

