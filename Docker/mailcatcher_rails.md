## DockerでMailCatcherを利用する(Rails)

### docker-compose.ymlを編集

``` yaml
version: "3"
services:
  mailcatcher:
    image: schickling/mailcatcher
    ports:
      - "1080:1080"
      - "1025:1025"
```

編集後

```
$ docker-compose up -d
```

これでhttp://localhost:1080 にアクセスするとMailCatcherのUI画面が表示されるはず。

まだメールは空だと思います。

---

### action_mailerの設定(smtpを使用するように)

config/environments/development.rbを編集する。mailcatcherの公式サイトのrailsプロジェクトのやり方を
丸コピしたらつまった。

``` ruby
  config.action_mailer.delivery_method = :smtp
  config.action_mailer.smtp_settings = { :address => "<Dockerコンテナ内からみたホストのIPアドレス>", :port => 1025}
  config.action_mailer.raise_delivery_errors = false
```
---

### Dockerコンテナ内からみたホストのIPアドレスを調べる

Dockerコンテナ内からみたホストのIPアドレスを調べるのに少し難渋した。

[Dockerコンテナ内からホストのIPアドレスをホスト側から知る](https://qiita.com/suketa/items/b42e8b92404634d36009)

他にもいろいろな記事が出てきたが、ipコマンドもrouteコマンドも打てない状況であれば、この方法で。

```
$ docker network list
(出力結果)
NETWORK ID     NAME                               DRIVER    SCOPE
<network_id>   docker-test_sample                 bridge    local
```

```
$ docker network inspect docker-test_sample | grep -i gateway
                    "Gateway": "<docker-host-ip>"
```

ここで問題。PowerShellではgrepコマンドが使えなかった。半べそ書きながら調べていたら下記のようなコマンドを見つけた。

```
$ docker network inspect dwango-test_sample | oss |Select-String "Gateway"
                    "Gateway": "<docker-host-ip>"
```

windowsならこれでいけるはず。

ここで拾ってきたIPアドレスを先ほどのconfig/environments/deveropment.rbの:adress部分に貼りつけ。


---

あとはアプリ側でメールを発信するような操作をすれば、localhost:1080のほうにメールが届いているはず。
