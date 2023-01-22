## TypeScript メモ

### 参考リンク
[TypeScriptの型入門 -Qiita](https://qiita.com/uhyo/items/e2fdef2d3236b9bfe74a)

- Union型 → `'foo' | 'bar'`みたいに複数の型の可能性を定義しているやつ。
- オブジェクトのあるプロパティは存在しない可能性もある、という場合 `name?: 'foo'`のように`?`を付けて定義することで表現できる。
  - JavaScriptでは存在しない値を読みに行こうとすると`undefined`が返される。そのため上記のような型定義の場合はname: `foo` | `undefined`という意味合いにもなる。
  - そのため、`name`プロパティを使用したいときには`undefined`のチェックをしてあげる必要がある。（`name !== undefined`）

