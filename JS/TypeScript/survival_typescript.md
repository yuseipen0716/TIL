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
