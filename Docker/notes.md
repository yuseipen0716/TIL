## Dockerいろいろメモ

### buildxを使ってみる

m1mac(arm64)でほかのメンバーと共有のイメージをbuildしようと思ったがうまくいかないことがあった。

マルチプラットフォーム対応でbuildできるように`docker buildx`というCLIプラグインがあるみたいなので、いろいろ調べてみよう。

> 参考: [マルチ CPU アーキテクチャー対応の活用](https://matsuand.github.io/docs.docker.jp.onthefly/desktop/multi-arch/)

