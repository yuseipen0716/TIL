## db/schema.rbでコンフリクトが発生したとき

merge先→developブランチ、現在の作業ブランチ→schemaブランチ

1. `git checkout develop`
2. `git pull origin develop`
3. `git checkout schema`
4. `git merge develop`
5. `git reset HEAD db/schema.rb`
6. `git checkout db/schema.rb`
7. `rails db:migrate`


> 参考: [コンフリクトしたschema.rbをきれいにマージする手順](https://qiita.com/jnchito/items/494a0499b808f109e0a8)
