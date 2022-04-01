## credentialsまとめ

秘密の文字列を平文で公開、またはバージョン管理システムに載せてしまうことは大変危険。

Railsではconfig/credentials.yml.encファイルに暗号化したYAMLファイルを保存している。

これを読み書きするには復号化用のキーが必要。このキーはrails newした時に生成される、`config/master.key`ファイルに書かれているので、これを外部に公開することだけは絶対にしてはいけない。

### credentialsの内容を確認

```
$ bin/rails credentials:show
```

何も設定していなければ

```
# aws:
#   access_key_id: 123
#   secret_access_key: 345
```

のように表示されるはず。

もしも環境を指定するのであれば

```
$ bin/rails credentials:show -e development
```
のようにしてあげる。他にもテスト環境、ステージング環境、本番環境をしていできる。

### credentialsの編集

``` 
$ EDITOR=vim bin/rails credentials:edit
```

今回は初めから記述されているawsのコメントアウトを解除してみる。

```
aws:
  access_key_id: 123
  secret_access_key: 345
```

### 秘密情報にアクセス

上記方法で設定した情報には

```
Rails.application.credentials.aws[:access_key_id]
```
というような形でアクセスできる。

consoleで試してみる。

```
# rails console

irb(main):001:0> Rails.application.credentials.aws[:access_key_id]
=> 123
irb(main):002:0> Rails.application.credentials.aws[:secret_access_key]
=> 456
```


