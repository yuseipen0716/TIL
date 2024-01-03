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
