## Webpacker:compileがちっともできない件

パーフェクトRORの4章でWebpackerを扱うため、Webpackerが標準でインストールされるRails6系にバージョンを落としてみた。

本ではまず、Webpackerの基本的な動作を確認するために、明治的にJSのビルドを行っている。

1. `$ rails new webpacker_sample`
2. `$ cd webpacker_sample`
3. `$ bin/rails g scaffold user name`
4. `$ bin/rails db:create`
5. `$ bin/rails db:migrate`
6. app/javascriptディレクトリの内容を確認
7. `ls 1FA app/javascript`でapp/javascript下にpacksとその下にapplication.jsがあることを確認
8. これから、明示的にJSのビルドを行う。`$ rails webpacker:compile`

Rails6.03で死ぬほど詰まって全然ここから進まなかった。

webpack not foundみたいなエラーが出て、色々といじったけれど解決せず。

Railsのバージョンを6.0.3→6.1.1に上げたら問題なくcompileできてしまった...
