## Windows10に別端末からリモートデスクトップを行えるようにする

Windows10Proでは「リモートデスクトップ」が利用できる。

自宅のPCに出先からリモートログインして作業したいと思ったので、重い腰をあげてやってみた。

Windowsの設定から「リモートデスクトップ」を有効にするだけでもいいっぽいけど、これだと3389ポート( RDPポート )を解放したままにしないといけない。

安全性を考えるのであれば、SSHのトンネリングを利用してリモートデスクトップを行う方法をお勧めしてもらった。

今回のメモの中ではリモートデスクトップで接続されるPCをサーバー側、外部から接続するPCをクライアント側として記述する。


### やること
- サーバー側の準備
  - SSHサーバーを立てる（別で書いたTIL参照）
  - リモートデスクトップの準備
    - リモートデスクトップを有効にする
    - 自動で解放されてしまう3389ポートを閉じる
- クライアント側の準備
  - サーバー側にSSH接続できるようにする
  - ( Macの場合 )Microsoft RemoteDesktopのアプリをインストール
  - SSH ローカルポートフォワーディングを利用してサーバー側にSSH接続
    - `ssh <サーバー側のユーザー名>@<サーバー側のIPアドレス> -i <鍵パス> -L <自身のポート番号>:<接続先サーバー名 or IPアドレス>:<接続先のポート番号>`
    - 例: `ssh username@xxx.xxx.xx.xxx -i ~/.ssh/id_ed25519 -L 3389:localhost:3389`
  - Microsoft RemoteDesktopでhostnameをlocalhostにして接続。
  - サーバー側のユーザー名、パスワードを入力
  - OK


詰まった点。ローカルネットワーク内でのリモートデスクトップはすぐにできたが、外部からの接続をするためにグローバルIP経由を指定してSSHしようとしたら、permission deniedのエラーがでた。

ルーターの設定で、ポートマッピングをちゃんと行えていなかったため失敗していたみたい。ポートマッピングを設定して、設定したポートを指定してSSHしたら行けた。

また今回、.ssh/configも設定した。下に接続例を書いておく。

```
Host remote
  Hostname xxx.xxx.xx.xxx( グローバルIP )
  User username
  Port xxxxx(ポートマッピングで指定したポート番号 22番ポート->xxxxx番のマッピング)
  IdentityFile ~/.ssh/id_ed25519
  LocalForward 3389 localhost:3389
```
この状態にしておけば`ssh remote`を実行するだけで、リモートデスクトップの準備が整う。

### 接続する
クライアント側のPCで「リモートデスクトップ接続」を立ち上げる。

コンピュータ名のところに`localhost:3389`と入力し、資格情報を入力して接続。

![image](https://user-images.githubusercontent.com/81737622/205023764-5cf69be1-6442-4652-b6dc-14b6d73fff6e.png)


### クライアント側の設定回り

リモートデスクトップをフル画面または、マルチディスプレイで行いたい場合は「リモートデスクトップ接続」>> 「オプション」>> 「画面」>> 「リモートセッションで全てのモニターを使用する」にチェックを入れる。

![image](https://user-images.githubusercontent.com/81737622/205023123-3eda11b3-691d-4964-99f2-01c2bfbde2bd.png)

### トラブルシューティング
- 「進行中のセッション～～～」で怒られた
  - 解決がむずかしかった。職場のデスクトップ(win10Pro)から自宅のデスクトップ(Win10Pro)に接続を試みた際に出たエラー
  - [こちら](https://github.com/stascorp/rdpwrap) を使用すれば解決するかも？
- 資格情報周りのエラー
  - [Remote Desktopで"お使いの資格情報は機能しませんでした"と表示される場合の対処法](https://qiita.com/Shion1305/items/45fb2d099db39febc1a0) 自分の場合は、こちらの記事にある方法で解決できた。



### 参考
- [【Windows Server 2019】SSH ポートフォワーディング（トンネリング）経由で、リモートデスクトップ接続する。](https://zapping.beccou.com/2021/08/21/using-ssh-port-forwarding-tunneling-about-remote-desktop-connection/#ssh-4)
- [Windows10に公開鍵認証でssh接続，Tensorboard表示の備忘録](https://zenn.dev/mitsu_h/articles/qiita-20201030-16a0dca9d43a50846ed0)
