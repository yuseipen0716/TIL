## Gitあれこれ

### staging前の変更を取り消したい

`git checkout HEAD <ファイルパス>`

---

### pushを取り消す方法

大前提→チーム開発において、すでにdevelopであったり、**masterにmerge済のpush**に対してやると、そのcommitに依存した実装がぶっ壊れてしまい、大変なことになるため絶対やっちゃダメ。

例えば、個人開発をしていて、このpushまで戻りたいという状況になった場合や、チーム開発の中でも自分だけが実装している開発ブランチ（まだmergeされていない状態）だったら使ってもいいかも。

- まずはreset先を指定するために、どのcommitをHEADにしたいか考え、該当する箇所のcommit_idを拾ってくる。
- `git reset <先ほど拾ってきたcommit_id> -hard`でソースファイルの変更分ごとreset
- 何か作業ブランチで変更を加えcommit
- `git push -f origin HEAD`として、force push（強制push）を行う

> 参考: [git resetコマンドで任意のpushまで巻き戻す](https://qiita.com/aki4000/items/bec93ba631a83b687fb4#%E5%BC%B7%E5%88%B6%E7%9A%84%E3%81%ABpush%E3%81%99%E3%82%8B)

---

### チーム開発メモ

- master(main,develop等、ブランチを切る元)ブランチに変更がないか確認(`git pull`)
- ルールにもよるが、チケットごとにブランチを切る（ブランチを切る前に必ずpullして変更ないか確認)
- developブランチから切る → `feature/oooo` というような名前の新規ブランチを作成して開発
  - `git checkout develop` → `git pull` → `git checkout -b feature/oooo`
- ローカルでadd, commit
- リモートリポジトリにpush
- PR出してレビューを受ける PR出すときのプルダウンメニューでDraftを選択すれば履歴を汚さない（PRの下書きみたいな感じ?)
- PR内に、先方からの要望に対して、自分がそれをどういう仕様として理解して実装したかも記載すると、レビュワーも見やすい（かも）
- develop→masterへのPRは(2022-05-17 本番リリース)のように、日付とやった内容がわかるように。

参考: [Githubでチーム開発するためのマニュアル](https://qiita.com/siida36/items/880d92559af9bd245c34)

---

### 直前のコミットメッセージを修正したい

commit したあとでcommitメッセージの誤り等に気づいた際(push前)にコミットメッセージを修正する方法

```
git commit --amend -m '<修正後コミットメッセージ>'
```


---

### developブランチにPRしようと思ったらconflict

[Git コンフリクト解決備忘録](https://qiita.com/crarrry/items/c5964512e21e383b73da)

また自分でもまとめないとね。



### コンフリクトが発生したら

### コンフリクト解消で痛い目をみた話



- >  [【gitコマンド】いまさらのrevert](https://qiita.com/chihiro/items/2fa827d0eac98109e7ee)
- >  [マージコミットと通常のコミットのただ一つの違い](https://udomomo.hatenablog.com/entry/2019/07/14/235323)
- >  [git merge 時は必ずマージコミットを作るようにする](https://neos21.net/blog/2017/06/18-01.html)


---
### git addでうっかり不要なファイルを追加し、気づかずcommit

ファイル名を指定して、`git add`したつもりだったので、 `git add`のあとに`git status`を見ることもなくpushしたら、不要なファイルが含まれていた。

```
git add app/models/ooo.rb
```
としたつもりが、

```
git add app/models/ooo.rb app/
```
となっていた！

app配下のファイルがすべてaddされていた。

念のため、`git add`のあとは`git status`してcommit対象が正しいか、確認しようと思いました。

---

### git push -> 間違ったuser.nameでpushしちゃった
仕事用のアカウントなどを作成した場合、git config --localで作業リポジトリのgit configを変更しておかないと、privateのgithubアカウントでpushしてしまうことになる。

その時に行った手順

- まずはPJのことを相談させてもらっている方に連絡
- git reset、git push -f等の操作を行っても大丈夫か確認をとる

以下、↑がOKだった場合の手順です。
- `git push -f origin <戻したいcommit_id>:<branch名>`
- `git reset --soft <commit_id>`
- `git config --local user.name "<right_user_name>"`
- `git config --local user.email "<right_user_email>"`
- `git config --local -l`
- `git config -l`
- 再度commit + push

---


### git pull origin master と git pull --rebase origin master

#### git fetch
pullの話の前にfetchの話を。

ローカルリポジトリの中にはローカルブランチ、リモート追跡ブランチがある。 

ローカルブランチには現在作業しているブランチのHEADまでの部分と、commitするファイルが入ったインデックス部分、それと現在作業中のワーキングツリーが含まれる。

リモート追跡ブランチというのはリモートリポジトリのコピーのこと。「origin/ブランチ名」で表される。

git fetchコマンドは、リモートリポジトリの最新のものをローカルリポジトリのリモート追跡ブランチにコピーするコマンド。

これを実行しただけでは、ローカルのファイルには何の変更も起こらない。

### git merge
git fetchでとってきた、リモート追跡ブランチの追加分をローカルブランチに追加する。

`git merge origin/master`というように記述。

この`git fetch`と`git merge origin/master`を組み合わせたのが、`git pull origin master`

### git pull --rebase origin master




---
### git revertに注意

