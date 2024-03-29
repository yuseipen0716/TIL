## RaspberryPiのセットアップと、最低限しておいた方がよいセキュリティ対策

### セットアップ
今回、RaspberryPi（以下、ラズパイ）のZero 2wという型を購入しました。

ラズパイを購入するのは初めてだったため、microHDMIケーブルなどの周辺機器もないような状況だったため、スターターキットのようなものを買いました。

付属のMicroSDカードにすでにRaspbianのディスクイメージが書き込まれた状態で家に届いたので、OSのイメージをSDカードに焼く手順については今回実施しておりません。

こちらについては、今後ラズパイを買った際にやってみたいと思います。

今回は割愛。

そんなわけで、スターターキットを買った場合についてメモしておく。

まずモニターやキーボードを本体に接続して、付属のSDカードを本体にさした状態で、電源ケーブルをつなげたら電源が入り、画面にOSのセットアップが麺が出力された。

特に難しいこともなく、案内に沿って進めばよい感じだった。

ラズパイZeroはUSBの端子が電源を刺す端子を除くと、1つしかなく、USB-A用のハブを所持していなかったため、少々面倒だった。(マウス操作が必要な場合はマウスを繋ぎ、キーボードでの入力が必要になったら、キーボードを繋げるという要領）

### 最低限しておくべきセキュリティ
今回、ラズパイを買ったのは、自宅内（あわよくば外部にも公開する）サーバとして運用してみたかったという理由がある。

そのため、今回は外部の機器を取り付けたりすることなく、単純に24時間、低コストで稼働するサーバ機というような認識で使用するため、そのために必要なセキュリティ対策は施したい。

1. rootログインを禁止

#### やりたいこと
- 自宅環境内の別端末からラズパイに対してSSH
- 自宅環境内の別端末からラズパイ宛てにHTTPリクエスト
- 外部（たとえばオフィス）のPCから自宅内のラズパイにSSH
- 外部から自宅内のラズパイにHTTPリクエスト

外部からのアクセスを許可するには自宅ネットワークのグローバルIPに向けてリクエストを送り、その際にリクエストの入り口を作るために特定のポートに穴をあけるような作業が必要になりそうなので、
一旦、自宅内でのアクセスが確実に行えることを確認してから手を出すことに。









