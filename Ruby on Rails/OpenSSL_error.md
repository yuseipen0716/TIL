## symbol not found in flat namespace '_SSL_get1_peer_certificate' で詰まった話

`rails new`をして新規プロジェクトを立ち上げようとしたら構築中に

```ruby
rails aborted!
LoadError: dlopen(/Users/<username>/Desktop/project/rails_app/blog_sample/blog_sample/.bundle/ruby/2.6.0
/gems/puma-5.6.2/lib/puma/puma_http11.bundle, 0x0009): symbol not found in flat namespace '_SSL_get1_peer_certificate' - 
/Users/<username>/Desktop/project/rails_app/blog_sample/blog_sample/.bundle/ruby/2.6.0/gems/puma-5.6.2/lib/puma/puma_http11.bundle

# みやすくするため、一部改行してあります
```

とエラーが出てしまい、webpackerの立ち上げ以降の作業が完了していない状態。

エラーの文をみるとpumaとかopensslがヒントか？

`ruby -ropenssl -e "puts %Q[\ \ \ Build: #{OpenSSL::OPENSSL_VERSION}\n Runtime: #{OpenSSL::OPENSSL_LIBRARY_VERSION}]"`というコマンドを打つと

```
   Build: OpenSSL 1.1.1l  24 Aug 2021
 Runtime: OpenSSL 1.1.1l  24 Aug 2021
```
---

errorのログには` symbol not found in flat namespace '_SSL_get1_peer_certificate'`と書いてある。この部分で検索をかけてみると[こちら](https://github.com/puma/puma/issues/2790)
のissueが見つかった。このissueの[この部分](https://github.com/puma/puma/issues/2790#issuecomment-1046264194) に解決策が紹介されていた。

OpenSSL1.1.1lにはSSL_get1_peer_certicicateが存在せず、そんなSSL1.1.1lで実行しているRubyを使用しているのに、OpenSSL3.0.xでPumaをコンパイルしているせいでエラーがでているみたい。

つまり、Rubyで使用しているOpenSSLのバージョンを3.0.xに上げればいいってことらしい。

そこで先ほどのリンク先で紹介されていた方法

1. ```
   brew reinstall openssl@3
   ``` 
2. ```
   rvm reinstall 3.1.0 --with-openssl-dir=`brew --prefix openssl`
   ```
   
   を実行。rvmの環境がきちんとできていなかったため、`command not found`のエラーが出て、やや苦戦した。
   
   やや年数たってますが、こちらの記事([RVMとは？(RVMを利用したRubyのインストール)](https://qiita.com/yunzeroin/items/f685c66a5455d354f6b6))を参考にしてrvmを使えるようにした。
   
   上記のOpenSSLのバージョンを調べるコマンドをたたくと
 ```
      Build: OpenSSL 3.0.1 14 Dec 2021
 Runtime: OpenSSL 3.0.1 14 Dec 2021
 ```
 と表示され、バージョンアップできたことが確認できた。
 
 ----
 
 満を辞して`rails new` 
 
 できた〜〜！



