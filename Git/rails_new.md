## Railsで作ったアプリをgithubで管理

- リモートリポジトリを作って、それを自分のローカルにクローン
- そのリポジトリの中で`rails new`でアプリを作って、変更を`git add .`

しようと思ったら次のようなエラーがでました。

```
error: '<app name>' does not have a commit checked out
fatal: adding files failed
```

`rails new`をすると、その中にも .git のファイルが作られてしまうみたいで、それが入れ子構造みたいになるためこのエラーが出てしまう？みたい。

次のようにしたらちゃんとリモートリポジトリにプッシュできたのでメモを残しておく。

### 手順
---

1. まずはgithubのほうでリモートリポジトリを作っておく。
    1. `rails new`するとREADME.mdも作成されるため、READMEファイルはこの時作らなくて良い
1. `rails new test`でtest というアプリを新規作成
2. `cd test`で先ほど立ち上げたアプリのディレクトリに入る。
3. `git init`
4. ↑で`git init`すると、`Reinitialized existing Git repository in (ディリクトリ名)`という実行結果が返ってくる。
    1. `rails new`するとその中で初期化の設定はされるらしい。
    2. ただ、もう一度`git init`しても特に問題ないみたい。
1. `git add .`
2. `git commit -m "first commit"`
3. `git remote add origin https://github.com/<user-name>/<repository-name>.git` でGithub上のリモートリポジトリとローカルリポジトリを紐付け **(ここ！ここがちゃんとできてなくて苦労した！)**
4. `git remote -v`でリモートリポジトリが正しく紐づけられているか確認
5. そういえば今いるブランチ確認してなかった。`git branch`
6. そうすると`master ~`と。待ってgithubで作られるデフォルトのブランチはmainだけど、今いるのはmasterになってる
    1. **今のGithubではデフォルトのブランチがmainになっている。master branchのままpushするとリモートリポジトリにブランチが二つできて、大変気持ち悪い**
8. `git checkout -b main`
9. 実行結果：`Switched to a new branch 'main'`
10. そうしたらあとはpush
11. `git push -u origin main`
12. できた
