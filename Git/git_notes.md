## Gitあれこれ

### staging前の変更を取り消したい

`git checkout HEAD <ファイルパス>`

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




