## コンフリクトを起こしたschemaを直す

#### 1. masterブランチを最新のバージョンにする
```
git checkout master
git pull origin master
```

#### 2. 作業ブランチ（feature/topic_a）に最新のmasterブランチをmerge
```
git checkout feature/topic_a
git merge master
```

ここでコンフリクトが出た旨のメッセージが出る。`git status`でどのファイルにコンフリクトが起きているか確認。

今回は、db/schema.rbのみにコンフリクトが発生している場合を想定する。

#### 3. db/schema.rbの変更を元に戻す
```
git reset HEAD db/schema.rb
git checkout db/schema.rb
```

#### 4. migration
```
bin/rake db:migrate
```

#### 5. 確認
これで一旦コンフリクトはなくなり、schemaもきれいになったはずなので、確認

```
git status
git diff db/schema.rb
```

#### 6. mergeの続行
```
git merge --continue
```

おしまい。
