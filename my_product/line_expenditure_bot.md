# LINE 家計簿 bot

## 概要
家計簿の管理を行うLINEbot

以前はGASで運用していたが、自宅のラズパイで運用する形に変えた。

### 構成
API -> Ruby on Rails APIモード
DB -> SQLite3
RaspberryPi4B 8GBモデルで運用。（Ubuntu 22.04.3 LTS (GNU/Linux 5.15.0-1042-raspi aarch64)）
Dockerで環境構築。

#### コンテナ
- webコンテナ（rails）
- nginxコンテナ
- certbotコンテナ（productionのみで動かす。SSL化のため）

