## RSpec setupメモ

何はともあれGemfileに追加

```
group :development, :test do
  ...
  gem 'rspec-rails' # 追加
end
```

`bundle install`

無事にinstall完了したら、

```
$ bundle exec rails g rspec:install
      create  .rspec
      create  spec
      create  spec/spec_helper.rb
      create  spec/rails_helper.rb
```

### FactoryBotのsetup
上記のRSpecのinstallが終わったら、取り合えず、rspecコマンドを実行することでテストを走らせることはできるようになる。

テストで使用するレコードを用意するために、FactoryBotを導入する。

Gemfileに追加

```
group :development, :test do
  ...
  gem 'factory_bot_rails' # 追加
end
```

この状態だと、

```ruby
user = FactoryBot.create(:user)
```

のようにしてFactoryBotを使用することができる。`spec/rails_helper.rb`に以下の設定を追加しておくと、FactoryBotというモジュール名を省略することができる。

```
# spec/rails_helper.rbに追記
config.include FactoryBot::Syntax::Methods
```

この設定追加後のuserのテストデータ作成
```ruby
user = create(:user)
```

### よりよく
`.rspec`ファイルに以下の設定を追記すると、テストの出力が見やすくなる。（好みによる気はする。）

.rspec
```
--format documentation
```
