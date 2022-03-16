# symbol not found in flat namespace '_SSL_get1_peer_certificate' で詰まった話

## Railsの新規プロジェクトがたちあげられなくなった

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

```
ruby -ropenssl -e "puts %Q[\ \ \ Build: #{OpenSSL::OPENSSL_VERSION}\n Runtime: #{OpenSSL::OPENSSL_LIBRARY_VERSION}]"
```
というコマンドを打つと

```
   Build: OpenSSL 1.1.1l  24 Aug 2021
 Runtime: OpenSSL 1.1.1l  24 Aug 2021
```
---

errorのログには` symbol not found in flat namespace '_SSL_get1_peer_certificate'`と書いてある。この部分で検索をかけてみると[こちら](https://github.com/puma/puma/issues/2790)
のissueが見つかった。このissueの[この部分](https://github.com/puma/puma/issues/2790#issuecomment-1046264194) に解決策が紹介されていた。

OpenSSL1.1.1lにはSSL_get1_peer_certicicateが存在せず、そんなSSL1.1.1lで実行しているRubyを使用しているのに、OpenSSL3.0.xでPumaをコンパイルしているせいでエラーがでているみたい。

つまり、Rubyで使用しているOpenSSLのバージョンを3.0.xに上げればいいってことらしい。

### issueに乗っていた解決策

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
 
 ---
 
 ## 追記
 
 これだと、Ruby3.1.0でバージョンが固定？されてしまった。2.6.9で動かしているプログラム利用したくて2.6.9にrvmで落とせと言われたからその通りにしたらerror
 
 `rvm install ruby-2.6.9`を実行しろという指示
 
 ```
$ rvm install ruby-2.6.9

rvm reinstall ruby-2.6.9

Gemset '' does not exist, 'rvm ruby-2.6.9 do rvm gemset create ' first, or append '--create'.
project % rvm mount -r 2.6.9
2.6.9 - #configure
Found remote file 
ruby-2.6.9 - #download
No remote url detected for ruby-2.6.9.
```

gemsetがないというので`--create`オプションを追加

```
$ rvm install ruby-2.6.9 --create     

ruby-2.6.9 - #removing src/ruby-2.6.9 - please wait
Searching for binary rubies, this might take some time.
No binary rubies available for: osx/12.2/arm64/ruby-2.6.9.
Continuing with compilation. Please read 'rvm help mount' to get more information on binary rubies.
Checking requirements for osx.
Updating certificates bundle '/opt/homebrew/etc/openssl@1.1/cert.pem'
Requirements installation successful.
Installing Ruby from source to: /Users/<user_name>/.rvm/rubies/ruby-2.6.9, this may take a while depending on your cpu(s)...
ruby-2.6.9 - #downloading ruby-2.6.9, this may take a while depending on your connection...
ruby-2.6.9 - #extracting ruby-2.6.9 to /Users/<user_name>/.rvm/src/ruby-2.6.9 - please wait
ruby-2.6.9 - #configuring - please wait
ruby-2.6.9 - #post-configuration - please wait
ruby-2.6.9 - #compiling - please wait
Error running '__rvm_make -j8',
please read /Users/<user_name>/.rvm/log/1647015154_ruby-2.6.9/make.log

There has been an error while running make. Halting the installation.
```
`Error running '__rvm_make -j8'`のエラーが出る。

### rbenvの方でバージョンを変えてみる
 
 rbenvの方でバージョンをダウングレードすると、利用しているOpenSSLがもとの1.1に戻ってしまった… 
 
 はて、どうしたものか
 
 ```
$ rbenv install 2.6.9 

Downloading openssl-1.1.1l.tar.gz...
-> https://dqw8nmjcqpjn7.cloudfront.net/0b7a3e5e59c34827fe0c3a74b7ec8baef302b98fa80088d7f9153aa16fa76bd1
Installing openssl-1.1.1l...
Installed openssl-1.1.1l to /Users/<user_name>/.rbenv/versions/2.6.9
```

rbenv等入れ直してみるが、同じようにRuby2.6.9をインストールするとopenssl1.1.1lが使用されてしまい、`rails new`でエラーとなる。

### OpenSSL3.0.xでRubyをbuildするために

[issue_by_MSP-Greg](https://github.com/puma/puma/issues/2790#issuecomment-1016958095) このissueにOpenSSL3.0.xでRubyがビルドできていない以上、このPumaでのエラーは解決できないようなことが書いてあったので、こちらを参考に`gem install openssl`してみる。

```
rails_app % gem install openssl
Fetching openssl-3.0.0.gem
Building native extensions. This could take a while...
ERROR:  Error installing openssl:
	ERROR: Failed to build gem native extension.

    current directory: /Users/<user_name>/.rvm/gems/ruby-3.1.0/gems/openssl-3.0.0/ext/openssl
/Users/<user_name>/.rbenv/versions/2.6.9/bin/ruby -I /Users/<user_name>/.rbenv/versions/2.6.9/lib/ruby/2.6.0 -r ./siteconf20220314-8798-1cb82kx.rb extconf.rb
```

これはこの間installしたrvmの残りカスなのだろうか。

→[ruby/openssl](https://github.com/ruby/openssl) こちらのREADMEを参考に、OpenSSLがインストール済みである場合の導入方法を試したら、gem installできた。

```
ruby -ropenssl -e "puts %Q[\ \ \ Build: #{OpenSSL::OPENSSL_VERSION}\n Runtime: #{OpenSSL::OPENSSL_LIBRARY_VERSION}]"
```
こちらで現在Rubyで使われているOpenSSLのバージョンを確認

```
   Build: OpenSSL 3.0.1 14 Dec 2021
 Runtime: OpenSSL 3.0.1 14 Dec 2021
 ```
 !!! できた？
 
 
→できてなかった

rails newすると、

```
rails aborted!
LoadError: dlopen(/Users/<user_name>/Desktop/project/rails_app/test_app/.bundle/ruby/2.6.0/gems/puma-5.6.2/lib/puma/puma_http11.bundle, 0x0009): symbol not found in flat namespace '_SSL_get1_peer_certificate' - /Users/<user_name>/Desktop/project/rails_app/test_app/.bundle/ruby/2.6.0/gems/puma-5.6.2/lib/puma/puma_http11.bundle
```

参照している.bundle の中のRubyが2.6.0になっているのは何か関係しているのだろうか。

### pumaをダウングレードしたら良いのだろうか…？

[こちらのissue](https://github.com/puma/puma/issues/2790#issuecomment-1030662172) をみると、pumaを5.5.2にしたら無事動いたというふうにも書いてありました。`rails new`した際に出るエラー分をみると、pumaは5.6.2を動かそうとしているようでしたので、もしかしたらこの方法も有用なのだろうか。

→pumaのダウングレードの方法がいまいちわからず→現在調査中

`gem install puma -v 5.5.2`でバージョン指定してpumaをインストールすることはできた。ただ、`puma --version`と打ってバージョンを確認しようとすると、

```
$ puma --version
/Users/<user_name>/.rbenv/versions/2.6.9/lib/ruby/2.6.0/rubygems/core_ext/kernel_require.rb:54:in `require': dlopen(/Users/<user_name>/.rvm/gems/ruby-3.1.0/gems/puma-5.5.2/lib/puma/puma_http11.bundle, 0x0009): symbol not found in flat namespace '_SSL_get1_peer_certificate' - /Users/<user_name>/.rvm/gems/ruby-3.1.0/gems/puma-5.5.2/lib/puma/puma_http11.bundle (LoadError)
```
というふうにエラーが出る。結局のところ、.rvm配下のパスを参照してしまっているのが原因か？？

```
$ gem environment
```

このように入力すると、gemの環境設定を呼び出せる

```
RubyGems Environment:
  - RUBYGEMS VERSION: 3.0.3.1
  - RUBY VERSION: 2.6.9 (2021-11-24 patchlevel 207) [arm64-darwin21]
  - INSTALLATION DIRECTORY: /Users/<user_name>/.rvm/gems/ruby-3.1.0
  - USER INSTALLATION DIRECTORY: /Users/<user_name>/.gem/ruby/2.6.0
  - RUBY EXECUTABLE: /Users/<user_name>/.rbenv/versions/2.6.9/bin/ruby
  - GIT EXECUTABLE: /opt/homebrew/bin/git
  - EXECUTABLE DIRECTORY: /Users/<user_name>/.rvm/gems/ruby-3.1.0/bin
  - SPEC CACHE DIRECTORY: /Users/<user_name>/.gem/specs
  - SYSTEM CONFIGURATION DIRECTORY: /Users/<user_name>/.rbenv/versions/2.6.9/etc
  - RUBYGEMS PLATFORMS:
    - ruby
    - arm64-darwin-21
  - GEM PATHS:
     - /Users/<user_name>/.rvm/gems/ruby-3.1.0
     - /Users/<user_name>/.rvm/gems/ruby-3.1.0@global
```

まだうまくいかない気もするけれど、今の状態で一度`rails new`してみる

```
$ bundle exec rails new test_app
...
...
       rails  webpacker:install
/Users/<user_name>/Desktop/project/rails_app/test_app/config/boot.rb:4:in `require': cannot load such file -- bootsnap/setup (LoadError)
	from /Users/<user_name>/Desktop/project/rails_app/test_app/config/boot.rb:4:in `<top (required)>'
	from bin/rails:4:in `require_relative'
	from bin/rails:4:in `<main>'
```
またエラーは出たけれど、はじめのころとは違うエラーになっている。

[mac Rails6 アプリ作成時エラーとサーバスタート時のエラーまとめ](https://qiita.com/miriwo/items/19c25034605c442e2fc7) こちらが参考にできるかな → x

上記のbootsnapはGemfileにbootsnapを追加してbundle installすることで解決したが、

```
$ bundle exec rails new test_app
~
~
~
       rails  webpacker:install
rails aborted!
LoadError: dlopen(/Users/makinomasayuki/vendor/bundle/ruby/2.6.0/gems/puma-5.5.2/lib/puma/puma_http11.bundle, 0x0009): symbol not found in flat namespace '_SSL_get1_peer_certificate' - /Users/makinomasayuki/vendor/bundle/ruby/2.6.0/gems/puma-5.5.2/lib/puma/puma_http11.bundle
```
結局上記のようなエラーがでてしまう。opensslのバージョンは3.0.1が使われるようにはなっているが、、

---

あんまり良くないことなのかもしれないけれど、ためしにpumaを4.3.6に落としてみる

→
```
$ bundle exec rails new test_app
~
~
~
✨  Done in 4.15s.
Webpacker successfully installed 🎉 🍰
```
！！！　

rails newで新しいプロジェクトの立ち上げに成功

作成したプロジェクトのディレクトリに移動して`bin/rails s`でサーバーを立ち上げてみる

```
project/rails_app/test_app/vendor/bundle/gems/puma-5.6.2/lib/puma/puma_http11.bundle, 0x0009): symbol not found in flat namespace '_SSL_get1_peer_certificate' - /Users/<user_name>/Desktop/project/rails_app/test_app/vendor/bundle/gems/puma-5.6.2/lib/puma/puma_http11.bundle (LoadError)
```

pumaのバージョンが5.6.2になっていて、また同じエラーが出ている。rails new で立ち上げたプロジェクトの中のGemfileもいじってあげないといけないのだろうか

`vim Gemfile`でpumaのバージョンを4.3.6にして`bundle install`

```
$ bin/rails s
=> Booting Puma
=> Rails 6.1.5 application starting in development 
=> Run `bin/rails server --help` for more startup options
Puma starting in single mode...
* Version 4.3.11 (ruby 2.6.9-p207), codename: Mysterious Traveller
* Min threads: 5, max threads: 5
* Environment: development
* Listening on tcp://127.0.0.1:3000
* Listening on tcp://[::1]:3000
```
サーバーたちあがりました。

bundle install で依存関係にあるgemのバージョンが変わったことによる影響か、railsのバージョンも当初と変わっているような気がするが、、

```
test_app % bundle exec puma --version
puma version 4.3.11
test_app % bundle exec rails -v
Rails 6.1.5
test_app % ruby -v
ruby 2.6.9p207 (2021-11-24 revision 67954) [arm64-darwin21]
```

ひとまず、rbenvでrubyのバージョンを操作して、railsの新しいプロジェクトを立ち上げるところまでは成功した。細かい点でのちのち修正も必要になるだろうから、そこはおいおいやっていく。











### 参考になるかもURL

[Install Ruby on Mac M1 Apple Silicon with Rbenv](https://blog.francium.tech/install-ruby-on-mac-m1-apple-silicon-with-rbenv-9253dde4e34a) 

[rbenv M1mac issue](https://github.com/rbenv/ruby-build/issues/1691#issuecomment-772224551)

[issue by MSP-Greg](https://github.com/puma/puma/issues/2790#issuecomment-1009044847) 


