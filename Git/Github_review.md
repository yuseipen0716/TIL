## コードレビューしてもらうための準備

個人で作業する分にはmainブランチで作業し続ければいいが、コードレビューをしていただく際などには、ブランチを切って、プルリクエストを送って、、、など作業が必要になる。

次にレビューしていただ機会に困らないようにまとめておく。

もしかしたら、人によってやり方の指定などがあったりするかもしれないので、○○なやり方で大丈夫でしょうか、と確認はすること。

- ブランチを切る。（レビュー用のブランチを作成）
    - ブランチを切りたいところのコミットIDを調べる。
    - ` git checkout (commit_id) -b tmp_for_review `(ブランチを新規作成して、そのブランチに移動する）
    - ` git push origin HEAD `
- `git checkout main `でmainブランチに戻る
- 何かファイルに変更を追加して、`add`, `commit`, `push`
- Githubのリモートリポジトリのページへ。
    - Pull requestsのタブをクリック
    - `New pull request`をクリック
    - main → tmp_for_reviewとなるようにPull requestを送る。　（つまり、compareがmain、baseがtmp_for_review)
