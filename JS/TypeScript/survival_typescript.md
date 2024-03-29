## サバイバルTypeScriptメモ

### memo
#### constアサーション
オブジェクトリテラルに`as const`キーワードを付けることで、プロパティをreadonlyにできる

```javascript
const obj = {
  foo: 'foo',
  bar: 'bar'
} as const;
```

こうすることで、例えば`obj.foo = bar`みたいな代入ができなくなる。

オブジェクトリテラルのプロパティごとに`readonlyを設定することもできる。

```javascript
const obj = {
  readonly foo: 'foo',
  bar: 'bar'
};
```

`as const`によるconstアサーションを行う場合、オブジェクトリテラル内にオブジェクトリテラルを持つ場合も再帰的にreadonlyにすることができる。

### typeof演算子 nullのとき
null型の値に対して`typeof`を使うと、`object`が返されるため注意が必要。

typeof null => objectを意識した実装の例（サバイバルTypeScriptから引用）

```javascript
function isObject(value) {
  return value !== null && typeof value === "object";
}
```

### unknown型について
unknown型はany型のような不明型で、どんな値でも代入できる。

any型との大きな違いは、そのまま使えないという点。any型は型の機能を放棄しているような状態であり、どのような値も代入できるし、それを使うこともできる。

unknown型はそのままでは使用できない。使用するには型を絞り込んであげる必要あり。

```javascript
// サバイバルTypeScriptより引用
const value: unknown = "";
// 型ガード
if (typeof value === "string") {
  // ここブロックではvalueはstring型として扱える
  console.log(value.toUpperCase());
}
```

このように`typeof`や`instanceof`で型を絞り込むような条件式を使うことを型ガードと呼ぶらしい。

型を絞り込んだあとは、その型として使用できる。

#### JSON.parse()の戻り値の型

JSON.parse()の戻り値の型はanyで返される。存在しないプロパティにアクセスしてしまうと、エラーが出てしまう

```javascript
const res: unknown = JSON.parse('...');
```

としてあげると、存在しないプロパティへのアクセスにコンパイル時に気づくことができる。

#### まったく異なる型への型アサーション
```javascript
const str = "a";
const num = str as number;
```
とすると、

```
Conversion of type 'string' to type 'number' may be a mistake because neither type sufficiently overlaps with the other. If this was intentional, convert the expression to 'unknown' first.
```
というようなエラーが出る。通常、型アサーションでは、まったく異なる型の指定はできない。

ここにunknown型を噛ますことで、上記の型アサーションでのエラーを回避することができるようだ。

```javascript
const str = "a";
const num = str as unknown as number;
```

※型アサーションは実際に値の型をキャストしているのではなく、TypeScriptにその型であると認識させているだけなので、型安全性の問題は残るとのこと。






