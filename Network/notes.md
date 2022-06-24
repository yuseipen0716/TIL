## ネットワーク関連メモ

PowerShellやCommand Promptを開いて`ipconfig`

```
Windows IP 構成


イーサネット アダプター vEthernet (Default Switch):

   接続固有の DNS サフィックス . . . . .:
   リンクローカル IPv6 アドレス. . . . .: xxxxxxxxxxxxxxxx
   IPv4 アドレス . . . . . . . . . . . .: xxxxxxxxxxxxxxxxxx
   サブネット マスク . . . . . . . . . .: xxxxxxxxxxxxxxx
   デフォルト ゲートウェイ . . . . . . .:

イーサネット アダプター イーサネット:

   接続固有の DNS サフィックス . . . . .: xxxxxxxxxxxxxxx
   IPv6 アドレス . . . . . . . . . . . .: xxxxxxxxxxxxxxx
   一時 IPv6 アドレス. . . . . . . . . .: xxxxxxxxxxxxxxx
   リンクローカル IPv6 アドレス. . . . .: xxxxxxxxxxxxxxx
   IPv4 アドレス . . . . . . . . . . . .: xxxxxxxxxxxxxxx
   サブネット マスク . . . . . . . . . .: xxxxxxxxxxxxxxx
   デフォルト ゲートウェイ . . . . . . .: xxxxxxxxxxxxxxx
                                          oooooooooooooooo

イーサネット アダプター vEthernet (WSL):

   接続固有の DNS サフィックス . . . . .:
   リンクローカル IPv6 アドレス. . . . .: xxxxxxxxxxxxxxx
   IPv4 アドレス . . . . . . . . . . . .: xxxxxxxxxxxxxxx
   サブネット マスク . . . . . . . . . .: xxxxxxxxxxxxxxx
   デフォルト ゲートウェイ . . . . . . .:
   ```
   というように、IP構成の一覧が表示される。
   
   この中の「ooooooooooooooooo」の部分をチェック。
   
   このIPアドレスにアクセスすると、LANとかの設定ページに移動する。
   
   ユーザー名やパスワードを要求される。ユーザー名は「admin」とか「user」であることが多い（たぶん指定されているはず）
   
   初期パスワードはルータに書いてあるはず。（後で変更しておきましょう。）

自分の環境の場合、「現在の状態」というページに飛ぶと、その中で「WAN側状態」という部分があり、その中にIPアドレスが表示されている。これがグローバルIP（らしい）

例えば、外部からSSHする場合などはこのIPを目指す。（ポート等の設定は必要。たぶんこの、ポートマッピングという部分。）




(memo)host only adapter
