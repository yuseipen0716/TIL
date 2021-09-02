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
