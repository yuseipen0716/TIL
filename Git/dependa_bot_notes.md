# DependaBotメモ
railsのプロジェクトで、gemのバージョンアップ関連のPRがDependaBotによって作られる場合、Gemfile.lockへの変更は行われないため、Merge前に以下のような手順で、Gemfile.lockの更新作業が必要だった。

1. local環境にDependaBotによって作られたbranckをfetch
2. docker compose upでエラーが出ることを確認
3. rm Gemfile.lock
4. docker compose upでエラーが出ないことを確認
5. git diff Gemfile.lockで変更内容を確認し、git add -> commit -> push
