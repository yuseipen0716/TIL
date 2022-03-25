## Railsで作ったアプリをgithubで管理

git ではまだデフォルトブランチがmasterで作成されてしまうが、githubではmainがデフォルトブランチとなっている。git config --global init.defaultBranch main`を入力してから`git init`で初期化すると、デフォルトがmainとなる。

- 新しいリポジトリを作成（READMEファイルも追加なし）
- `bundle exec rails new <app_name>`
- `cd <app_name>`で作成したアプリケーションのディレクトリに入る。
- `git init`
- `git remote add origin <remote repo url>`
- `git branch -M main`  いらんかも
- `git add .` → `git commit -m 'initial commit'
- 'git push -u origin main`

これでいけるはず！


