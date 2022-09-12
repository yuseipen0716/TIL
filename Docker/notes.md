## Dockerいろいろメモ

### buildxを使ってみる

m1mac(arm64)でほかのメンバーと共有のイメージをbuildしようと思ったがうまくいかないことがあった。

マルチプラットフォーム対応でbuildできるように`docker buildx`というCLIプラグインがあるみたいなので、いろいろ調べてみよう。

> 参考: [マルチ CPU アーキテクチャー対応の活用](https://matsuand.github.io/docs.docker.jp.onthefly/desktop/multi-arch/)

### PC起動時に立ち上がってしまうコンテナを落とす

PCを起動すると自動で立ち上がるコンテナがある。それが立っているせいで、必要なコンテナが立ち上がらないことがある。

そもそも自動で立ち上がらないようにする方が先かもしれないが、shellのワンラインで、最初に立ち上がったコンテナをひとまず落とすことができたので、メモに残しておく。

```
docker ps | awk '{print $1}' | sed -e 's/CONTAINER//g' | xargs docker stop
```
