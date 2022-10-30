## Windows10でSSHサーバを立てる

### 準備
#### OpenSSHサーバーの用意
OpenSSHサーバーが入っていないことが多いと思うので、インストールしておく。

- 設定 >> アプリ >> オプション機能 >> OpenSSHサーバーをインストール

#### ポート開放
以下の作業、大概管理者権限が必要

`netsh advfirewall firewall add rule name="sshd" dir=in action=allow protocol=TCP localport=22`

#### start sshd

- sshd install
- Start-Service sshd
- Set-Service sshd -StartupType Automatic

とりあえずここで接続できるかテストしてみる。

ラズパイやらmacなどの別端末から`ssh <ユーザーネーム>@<ホスト名(IPアドレス)>`

defaultだとpassword認証だけど、たぶんこれで接続できるはず。

### 公開鍵認証でのログインを行えるように
セキュリティの観点からもパスワード認証より、公開鍵認証の方が好ましい。sshdの設定ファイルを編集する。
 
- sshdを終了 `Stop-Service sshd`
- `C:\ProgramData\ssh\ssh_host_***key`と`C:\ProgramData\ssh\ssh_host_***key.pub`を削除( また後で、sshdを起動すればこれらの鍵ファイルは作成される。 )
- `gsudo nvim C:\ProgramData\ssh\sshd_config`
- `PubkeyAuthentication yes`のコメントアウトを解除
- 最後の方に記載されている↓の部分をコメントアウト
```
Match Group administrators
       AuthorizedKeysFile __PROGRAMDATA__/ssh/administrators_authorized_keys
```
- sshdを起動 `Start-Service sshd`

#### クライアント側の設定
- keyペアを作成し、公開鍵をサーバー側の~/.ssh/authorized_keysに登録

これでsshできるはず




