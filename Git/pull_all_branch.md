## リモートリポジトリの全てのブランチをpullする方法

リモートリポジトリからgit cloneして、`git branch`するとmainブランチしか見えない。

全てのブランチを見られるようにする方法がないか探していたら、[こちら](https://qiita.com/muraikenta/items/e590a380191971f9c4c3)の記事に辿り着いた。

```
for remote in `git branch -r`; do git branch --track ${remote#origin/} $remote; done
```
```
git fetch --all
```
```
git pull --all
```
