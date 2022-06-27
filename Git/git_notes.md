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




