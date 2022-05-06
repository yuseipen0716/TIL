## Gitあれこれ

### staging前の変更を取り消したい

`git checkout HEAD <ファイルパス>`

### チーム開発メモ

- master(main,develop等、ブランチを切る元)ブランチに変更がないか確認(`git pull`)
- ルールにもよるが、チケットごとにブランチを切る（ブランチを切る前に必ずpullして変更ないか確認)
- developブランチから切る → `feature/oooo` というような名前の新規ブランチを作成して開発
- ` 
- ローカルでadd, commit
- リモートリポジトリにpush
- PR出してレビューを受ける
