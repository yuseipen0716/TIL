## windowsでsshを使う準備

ssh-add でデフォルトのsshキーを追加しようと思ったら`unable to start ssh-agent service, error :1058`のエラーが出た。

ssh-agentが無効になっていることが原因らしかった。

1. windowsの設定 >> アプリ >> オプション機能 >> + 機能の追加 >> 「ssh」で検索 >> OpenSSHサーバー をインストール
2. `gsudo Start-Service sshd`
3. `gsudo Set-Service -Name ssh-agent -StartupType Manual`
4. `gsudo Start-Service ssh-agent`
5. `ssh-add <キーのパス>`

でキーの追加ができた。

このあたりの記事、併せて読んでおこうね。

- > [ssh公開鍵認証設定まとめ](https://qiita.com/ir-yk/items/af8550fea92b5c5f7fca)
- > [unable to start ssh-agent service, error :1058](https://qiita.com/tmak_tsukamoto/items/c72399a4a6d7ff55fcdb)


