## Minitestについて

Minitestを使ったテストコードの基本形

```ruby
require 'minitest/autorun'

class SampleTest < Minitest::Test
  def test_sample
    assert_equal 'RUBY', 'ruby'.upcase
  end
end
```
