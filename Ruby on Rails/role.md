## 権限回りのgem等について

権限回りの機能実装のためにpunditを使用する機械があったのでちょっと復習。

### 認証と認可について

認証→あなたがだれか(通信の相手が誰(何)であるかを確認すること)
認可→なにが許可されているか(とある条件において、リソースのアクセス権を与える)

参考: [よくわかる認証と認可](https://dev.classmethod.jp/articles/authentication-and-authorization/)

### punditとは

権限回り(認可まわり)の機能を作成するのに便利なgem

参考記事: [Punditを使って権限を管理する](https://qiita.com/senou/items/e28ff548e93ad0eeed3f)

