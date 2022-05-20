## controllerの変更が全然反映されなくて困ったはなし

indexのviewsで出る記事の出しわけをしたくて、controllerの@articlesにクエリを追加したが、うんともすんとも反映されない。

これちゃんとcontroller読まれているのかなってcontrollerのファイルにraiseを差し込んでも例外吐かない。

調べてみたら、Docker上でRailsを動かしている人で同じ現象が起こっている人がいた。

### 解決策

config/environment/development.rbを修正

`config.reload_classes_only_on_change = false`を追記もしくは

```
- config.file_watcher = ActiveSupport::EventedFileUpdateChecker
+ config.file_watcher = ActiveSupport::FileUpdateChecker
```
というように変更してあげるとcontrollerの変更がすぐに反映される。
