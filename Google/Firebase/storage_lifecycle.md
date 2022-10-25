## Firebase Storage オブジェクトのライフサイクルについて

Firebase Storageにファイルを保存する仕組みを作成したら、もう一つ考えないといけないことがある。

そのファイルをどのくらいの期間保持しておくか、などのルールである。

無料プランを使用していれば問題ないとは思うが、一定容量を超えたときに従量課金が始まってしまうと大変なので、Storage内のファイルを
定期的に削除する仕組みを作りたいと思った。

↓ こちらの記事を参考にして試してみた。

[Firebase Storageのファイルを定期的に自動で削除する](https://qiita.com/SatoTakumi/items/4a93def7f50fedf859ad)

Firebase StorageはGoogle Cloud Platform（以下、GCP）のCloud Storageが使われている。そのため、GCPのCloud Storageの
[オブジェクトのライフサイクル管理](https://cloud.google.com/storage/docs/lifecycle?hl=ja#age) を設定してみた。

まずは[こちら](https://cloud.google.com/storage/docs/gsutil_install?hl=ja#windows) からgsutilをinstallする。

Google Cloud SDKの初期設定を行った後、任意のディレクトリに`lifecycle_config.json`を作成する。

conditionのところに記載したageなどの条件は上記の公式ドキュメントに載っている。
( 以下は、30日経過したファイルが削除される設定 )

```
# lifecycle_config.json
{
  "lifecycle": {
    "rule":
    [
      {
        "action": {"type": "Delete"},
        "condition": {"age": 30}
      }
    ]
  }
}
```

lifecycle_config.jsonが作成できたら、ライフサイクルを有効にする。

```
$ gsutil lifecycle set lifecycle_config.json gs://xxxx(自分のバケット)

=> Setting lifecycle configuration on gs://xxxx/...
```

変更が反映されたか確認するにはgetコマンドを実行する
```
$ gsutil lifecycle get gs://xxxx

=> {"rule": [{"action": {"type": "Delete"}, "condition": {"age": 30}}]}
```
