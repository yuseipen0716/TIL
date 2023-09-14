## privateメソッドのテスト
本来privateメソッドはそのクラスの処理の中でのみ使われるようなメソッドで、基本的にテストはあまり書かない気もするが、privateメソッドのunitテストを書きたいことがあり、調べた。

### `send`メソッドを使用する
Rubyのオブジェクトは`send`メソッドをもっており、`send`メソッドを使用して、メソッド呼び出しを行うことができる。

```ruby
class SomeModel < ActiveModel
  def initialize(hoge)
    @hoge = hoge
  end

  private

  def some_private_method
    # ...
  end
end
```

というようなSomeModelがある場合、このclass内で定義された`some_private_method`をテストするには

```ruby
RSpec.describe SomeModel, type: :model do
  describe 'some_private_method' do
    let(:some_model) { SomeModel.new }

    it 'does something' do
      result = some_model.send(:some_private_method)
      expect(result).to eq(expected_value)
    end
  end
end
```

ざっくり、上記のような感じで書くことができる。

### privateメソッドが引数をとるとき
`send`メソッドで

```ruby
result = some_model.send(:some_private_method)
```

としている部分を

```ruby
result = some_model.send(:some_private_method, args1, args2)
```

のようにしてあげればよい。
