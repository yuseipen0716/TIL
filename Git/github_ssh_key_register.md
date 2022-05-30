## SSHキーを生成してGitHubに登録する

PCを変えたり、いろいろと環境が変わったタイミングで、新しくSSHキーを作ってGitHubに登録するが、いつもやり方を忘れたり間違えたりするのでメモを残しておく。

(※ 最近、ステージングサーバに入るための認証の際に、公開鍵を新しく作成して、うっかりgithubで登録してある公開鍵を上書きしてしまったようで、
リモートリポジトリに接続できなくなってしまった。猛烈に反省中)


あくまでGitHubの公式ドキュメントが大元だと思うので、そこを参考にしましょうね。

> 参考: 
> - [GitHub アカウントへの新しい SSH キーの追加](https://docs.github.com/ja/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account)
> - [[Git]SSH Keyのファイル名を変更したときの対応](https://furudate.hatenablog.com/entry/2013/12/16/190317)
> - [GitHubでssh接続する手順\~公開鍵・秘密鍵の生成から\~](https://qiita.com/shizuma/items/2b2f873a0034839e47ce)

まず、ssh-keygenのコマンドであったり、そもそもの公開鍵暗号方式について学ぶ必要がありそう。

こちら([ssh-keygenコマンドの使い方](https://hana-shin.hatenablog.com/entry/2021/12/21/202454))の記事を読んでみよう。

~SSHキーを作る際には名前を付けよう~ → 基本的には1マシン1キーでいい


