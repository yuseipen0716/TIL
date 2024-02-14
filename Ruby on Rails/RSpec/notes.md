## RSpec関連 memo
### テストをpendingしたい
何らかの理由でテストが落ちてしまうが、CIを通す必要がある場合などにtestをpendingしたいケースがあるかもしれない。

以下のように`x`を追加すると、そのテストはpendingされる。

```Diff
- context 'this is pending test' do
+ xcontext 'this is pending test' do
  it 'テストは実行されない。' do
    expect(hoge).to eq('hoge')
  end
end
```

#### pendingした理由をテストの出力に追加したい
その場合は`pending`メソッド（または、`skip`メソッド）を使用するみたい。

```ruby
context 'this is pending test' do
  it 'テストは実行されない。' do
    pending 'この機能はまだ実装されていないためpending'
    expect(hoge).to eq('hoge')
  end
end
```
