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

`pending`メソッドだと、expectation実行されて、テストが落ちない場合にエラーが出る！

skipだとそのような挙動ではなく、純粋にpendingしてくれるっぽい

itブロック内に配置しないと、テストのスキップはしてくれない...?

#### reload
```ruby
describe 'update article' do
  let(:update_method) { described_class.update_method(article) }
  let(:article) { create(:article) }
  ...

  it '記事のステータスが更新されたこと' do
    update_method
    expect(article.status).to eq(:after_update_status)
  end
end
```

大分端折っているが、上記のようなテストを書いていた場合、`update_method`の内部で引数で渡した`article`のstatusを更新しているとする。

letで宣言した`article`は、`update_method`呼び出し時に引数として渡されるため、その部分で遅延評価され、Articleモデルのレコードが作成されている。

expectの部分で、`article`のステータスが変更されたことを期待しているが、恐らくこれは失敗する。

変数articleは遅延評価された時点の状態のものをキャッシュし、そのテストケース内で再度articleが呼び出された場合はキャッシュを利用するようなので、ステータスの変更のテストができない。

```ruby
expect(Article.last.status).to eq(:after_update_status)
```

のようにすれば期待された結果が得られるとは思うが、この部分をどうしてもarticleという変数名でアクセスしたい場合は、`reload`を使用する。

```diff
describe 'update article' do
  let(:update_method) { described_class.update_method(article) }
  let(:article) { create(:article) }
  ...

  it '記事のステータスが更新されたこと' do
    update_method
-    expect(article.status).to eq(:after_update_status)
+    expect(article.reload.status).to eq(:after_update_status)
  end
end
```

こうすると、再度DBへアクセスし、最新の状態の`article`が取得されるため、テストが通るようになるはず。


