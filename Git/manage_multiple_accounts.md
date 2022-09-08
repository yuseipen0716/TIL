## 複数アカウントを管理する場合

会社用で別アカウントを作成して、そのアカウントを用いてcloneやpushを行う必要がある場合のメモ。

private用のgithubで登録されている公開鍵を会社用のアカウントに登録しようとすると、エラーが出る。

### キーペアを別で作成
- `cd ~/.ssh`
- `ssh-keygen -t ed25519 -C "email-for-work@example.jp" -f <key-name>`
- 公開鍵をgithubに登録
- ~/.ssh/configを編集する
```
Host github.com.work
  HostName github.com
  User git
  IdentityFile ~/.ssh/<secret_key_work>
  
Host github.com.private
  HostName github.com
  User git
  IdentityFile ~/.ssh/<secret_key_private>
```
- `ssh -T git@github.com.work`で疎通確認
- `Hi! <github.user.name.for_work> xxxxxxxxx`というようなメッセージが返ってきたらOK
- git configの編集(pushするときなどに、プライベートのアカウントでpushしてしまうミスを犯しました。要注意)
- `git config -l`で現在の設定を一応確認
- `git config --local user.name "user_name_for_work"` 該当PJのディレクトリで実行
- `git config --local user.email "user_email_for_work"` 該当PJのディレクトリで実行
- `git config -l`, `git config --local -l`で設定された内容を確認
