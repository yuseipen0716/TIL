## Ruby エラーまとめ

### 目次
---

- Minitestでtestが走らない
- NoMethodError: undefined method 'method_name' for nil:NilClass
- eachなどを使っているときに出るnilClassのエラー
- 続きはここから

### エラーの詳細
---

#### Minitestでtestが走らない

```
.
├── lib
│   ├── convert_hash_syntax.rb
│   ├── convert_length.rb
│   ├── fizz_buzz.rb
│   ├── gate.rb
│   ├── rgb.rb
│   ├── sample.rb
│   └── ticket.rb
└── test
    ├── convert_hash_syntax_test.rb
    ├── convert_length_test.rb
    ├── fizz_buzz_test.rb
    ├── gate_test.rb
    └── rgb_test.rb
```
となっている時、terminal上でルートディレクトリに入って`ruby test/rgb_test.rb`と打てば

```
ruby-book % ruby test/rgb_test.rb
Run options: --seed 61528

# Running:

..

Finished in 0.000359s, 5571.0316 runs/s, 16713.0947 assertions/s.
2 runs, 6 assertions, 0 failures, 0 errors, 0 skips
```
といった風にテストが実行される

そんな中、

```
Traceback (most recent call last):
	2: from test/gate_test.rb:2:in `<main>'
```
とテストの画面にもならないエラーが出た。

ソースコードを確認したら、**メソッド内の`end`が抜けていた!** 他にもスペルミス等でも起こるようなので、同じようなエラーが出た時にはソースコードを確認してみよう。

---

#### NoMethodError: undefined method 'method_name' for nil:NilClass

`NoMethodError: undefined method 'new' for nil:NilClass`

Minitestを実行すると、上記のようなエラーが出た。

今回は'new'メソッド、つまりinitializeメソッドが何かおかしいみたい。

エラー文の下に何行目がいけないのか、書いてあるから確認してみる。

テストコードの中で`Ticket.new(fare)`　とするところがあって、そこの記述が `**t**icket.new(fare)`となっていた！超凡ミス！

この部分を修正しましたら、無事テストも通りました。

---
