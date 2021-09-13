## Minitestについて

### Minitestを使ったテストコードの基本形

```ruby
require 'minitest/autorun'

class SampleTest < Minitest::Test
  def test_sample
    assert_equal 'RUBY', 'ruby'.upcase
  end
end
```

クラスの名前はは○○TestというようにTestで終わる。または、Testで始まる名前をつけることが多いみたい。

テストファイルの名前はsample_test.rbのようにクラスに合わせた名前にして、スネークケースで記述。

クラスの名前はSampleTestのようにキャメルケースで。


### 使用するMinitestの検証メソッド

```ruby
# aがbと等しければパスする
assert_equal b, a  #(順番注意！）

# aが真であればパスする
assert a

# aが偽であればパスする
refute a
```
他にも色々と検証メソッドはあるみたいだけど、まずはこれだけ覚えとく。

プログラム本体とテストコードはファイルを分けるのが一般的

テストコードのファイルの中で

```ruby
require 'minitest/autorun'
require './プログラムのファイルが入っているディレクトリ/プログラムファイル'
```

というようにして、テストコードのファイルから、テストしたいプログラムのファイルを読み込む。

### テスト駆動開発の開発サイクル
---
1. **先にテストを書いて失敗させる**
2. **テストがパスするような最小限のコードを書く。**
3. **リファクタリングする**

１では最初にテストコードから書き始める→テストをしてみる→失敗するのを確認→ロジックらしいロジックのない固定の値を実装する（仮実装、Fake it）  
これでテスト対象のメソッドとテストコードがちゃんとリンクしているのかを確認する。  
これをせずに、もし↑のどこかで間違えていたら、きちんとしたコードを書いても永遠にテストがパスできない状況になってしまう。

↑で書いた、ロジックのない固定の値は一つだけでなく、2つ目も書いた。（三角測量というみたい）  
テストの対象が1つしかないと、対象のメソッドが仮実装のままなのか、ロジックを実装しているのかの判別がつかないから。

### Minitestを動かした時に出たエラー

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


