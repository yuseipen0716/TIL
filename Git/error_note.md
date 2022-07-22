## 出会ったエラーたち

### 目次
---
- error: failed to push some refs to '<repository>'
- error: pathspec '<commit-message>'' did not match any file(s) known to git
- sshの設定をしたのに毎回user_nameとpasswordを聞かれる件
  
---
  
### push した時にerror: failed to push some refs to '<repository>'
  
git push すると下記のようなエラーがでた。

```
! [rejected]        main -> main (fetch fast)
error: failed to push some refs to '<repository>'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. Integrate the remote changes (e.g.
hint: 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```
  
#### 解決方法

とりあえず、fetch、mergeをしないといけないみたい。
  
1. `git fetch origin`
1. `git merge --allow-unrelated-histories origin/main`
1. `git push origin main`
  

  
追記：　これたぶん、terminal上でgit commitとかしてるんだけど、README.mdとかのファイルGitHub上でいじって、mainブランチの内容が、terminalで追跡しているものと異なるよ！っていうエラーかな？
  
fetch と mergeを合わせたpullコマンドで解決する気がします。
  
### error: pathspec '<commit-message>'' did not match any file(s) known to git
  
windows10の環境下でgit add .してcommit しようとしたら出た
  
調べていたら解決策としてcommitメッセージはシングルクオートでなくダブルクォートで囲いましょうとあった。
  
まさにその通りでした。
  
  ---
  
  ### sshの設定をしたのに毎回user_nameとpasswordを聞かれる件
  
  git pullするときなど、user_nameやpasswordを使用した認証はもう終了しているとのことだったが、いまだにgit pullするとuser_nameを聞かれるような状況で、うまくpullできなかった
  
  ```
  $ git remote -v
  ```
  
  を実行すると、今どのような方法でリモートリポジトリにアクセスしているかが表示される。
  
  `https----`みたいな表示の時は、http接続している。`git@github.com-----`な時はssh接続している。
  
  今回はsshのキー登録も済んでいるので、後者の方法で接続したい。そうすればuser_nameやpassフレーズを聞かれることもなかろう。
  
  すでに、どちらかの方法で接続方法が設定されている場合は
  
  ```
  $ git remote set-url origin git@github.com:USERNAME/REPOSITORY.git
  ```
  という風に実行(今回はhttps接続→ssh接続に切り替え)
  
  その後、`$ git remote -v`を実行するとssh接続に切り替わっていることが分かった。
  
  git pullしたら、問題なく行えました。
  
  #### 参考
  
  [リモートリポジトリを管理する](https://docs.github.com/ja/get-started/getting-started-with-git/managing-remote-repositories)
  
  ---
  
  ### git pull => Not possible to fast-foward のエラーが出てpullできない
  
  Aブランチから切ったBブランチにおいて、Aブランチの新規変更分をBに反映させたいときに、`git pull origin A`しようとしたら
  
  ```
  fatal: Not possible to fast-forward, aborting.
  ```
  
  みたいなエラーが出てpullできないことがあった。
  
  git pullしてリモートから取り込もうとしているコミットより新しいコミットをローカルリポジトリでしてしまっているのが原因？
  
  調べてみると、`git fetch`と`git rebase`を実行してあげたらいいらしい。
  
  ```
  git pull origin A --rebase
  ```
  としてあげると無事pullできた。この部分でコンフリクトが発生してしまう場合もある。
  
  
  
  
  



