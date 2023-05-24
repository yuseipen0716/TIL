## 時間を扱うテスト書くときに注意すること
最近、時間に依存する処理のテストを書いた。

具体的に言うと、1週間前に実行済みであれば、この処理は実行しないみたいな処理が本当に実行されないかの検証をしたかった。

実行した日付・時刻を`exec_at`というカラムに保存しているとする。

before句の中で必要なデータのセットアップをし、その中で`exec_at`を`1.week.ago.change(hour: 12)`のように設定しておき、`Time.zone.now`と比較するようなテストにする。

やりたいことは何となく合っていそうだが、テストが落ちたり、通ったり不安定。

よくよく見てみると、現在時刻に依存してしまっているため、テストを実行する時刻によってテストが落ちることがあった。

## travel_toを使う
調べてみると`travel_to`という機能を使えば、テストの中で固定の時間を設定できるようだった。

`travel_to`を使用するのに、gemのinstallなどは必要なく、RSpecのhlperのファイルに設定を記載すればOKなようだった。

### travel_toを使用できるように
```ruby
spec/rails_helper.rb
RSpec.configure do |config|
  config.include ActiveSupport::Testing::TimeHelpers
end
```

### Spec内での使い方
```ruby
it '一週間以内に実行している場合は処理が再度実行されないこと' do
  travel_to Time.zone.local(2023, 5, 2, 17, 0, 0) do
    # このテストケース内では時刻が2023年5月2日17時に固定される。
    # 何らかの処理
    expect(hogehoge).to eq(hoge)
  end
end
```

上のサンプルコードではitの内部で使用しているが、beforeのブロック内でも使用できる。
```ruby
before do
  travel_to Time.zone.local(2023, 5, 2, 17, 0, 0) do
    # beforeブロック内の時刻が2023年5月2日17時に固定される。
    # testに必要なデータのsetup
  end
end
```

今回自分は、`base_time`というような変数を定義し、1つのSpecファイルで基準となる時刻を固定した。

```ruby
let(:base_time) { Time.zone.local(2023, 5, 2, 17, 0, 0) }

# これにより、上記のサンプルコードでbase_timeを使いまわせる

before do
  travel_to base_time do
    # beforeブロック内の時刻が2023年5月2日17時に固定される。
    # testに必要なデータのsetup
  end
end

it '一週間以内に実行している場合は処理が再度実行されないこと' do
  travel_to base_time do
    # このテストケース内では時刻が2023年5月2日17時に固定される。
    # 何らかの処理
    expect(hogehoge).to eq(hoge)
  end
end
```
    
