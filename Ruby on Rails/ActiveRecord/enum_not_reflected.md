## カラムのstatusをenumで管理

statusみたいなカラムをintegerで定義してpublishedというstatusを0、draftを1のように定義するとき、
モデルファイル内で

``` ruby
enum status: {published: 0, draft: 1}
```

というように定義する。

他のカラムと同名のstatusを定義することもあるだろう。

``` ruby
enum status: {published: 0, draft: 1}, _prefix: true
```

としてあげる。

### enumの定義を更新したのに反映されない!!

statusを追加しようと思い、

``` ruby
enum status: {published: 0, draft: 1, droped: 2}, _prefix: true
```

というようにしたが、console等で新しいstatusが反映されているかどうか確かめたところ、反映されていない。

他のコードが影響しているのかと思っていろいろ見て回ったが、わからず。

**→ dockerコンテナを再起動したら直った。**

