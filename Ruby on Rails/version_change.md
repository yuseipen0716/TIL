## Ruby や Ruby on Railsのバージョン変更

### Ruby

1. 現在のバージョンを確認
    - `$ ruby -v`
2. インストール可能なバージョンを確認
    - `$ rbenv install --list`
3. 目的のバージョンをインストール
    - `$ rbenv install 3.1.0`
4. 利用するバージョンを変更
    - `$ rbenv global 3.1.0`
5. コンソール上で動いているバージョンの確認
    - `$ rbenv versions`
6. rehashする
    - `$ rbenv rehash`

---

### Rails

1. 現在使えるRailsの確認
    - `$ gem list rails`
2. 使用したいRailsのバージョンを指定してインストール
    - `$ gem install rails -v 6.1.0`
3. 現在使えるRailsの確認をすると、今インストールしたものが追加されている
    - `$ gem list rails`
4. バージョンを指定してプロジェクトを立ち上げる
    - `$ rails _6.1.0_ new sample`

[Rails アップグレードガイド](https://railsguides.jp/upgrading_ruby_on_rails.html)

### Docker環境などで環境を作っているプロジェクトでRubyのversionを指定する
.ruby-versionというファイルを作り、
```
3.1.0
```

のような記載をしておく。

これをプロジェクトルートにおいておくと、

ruby -vしたときに

```
$ ruby -v
rbenv: version `3.1.0' is not installed (set by /home/yuseipen0716/test-pj/.ruby-version)
```
のような警告が出る

```
rbenv install 3.1.0
```
をし、規定のversionをinstall

### rbenvとruby-buildのupgrade
最新のrubyにversion changeしようとしたらrbenvが最新になっていなくてできなかった。

apt upgradeしても最新版にならなかったので、調べた。自分でrbenvのrepoをcloneすればいいみたい。

```
rm -rf .rbenv
git clone https://github.com/sstephenson/rbenv.git .rbenv
```

ruby-buildも

```
git clone https://github.com/sstephenson/ruby-build.git ~/.rbenv/plugins/ruby-build
```

.bashrcに以下のような記述があるか、念のため確認

```
export PATH="~/.rbenv/shims:/usr/local/bin:$PATH"
eval "$(rbenv init -)"
```

あとは、shellを再起動して、rbenv -vしてversion確認。


