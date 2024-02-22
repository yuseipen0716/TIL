## Capybara Tips
feature specを書く際に調べたことなどのまとめ。

### 要素の検索
#### placeholderの文字列をキーに要素を探したい
```ruby
feature '検索機能のテスト' do
  scenario 'ユーザーが検索を実行する' do
    visit root_path # あるいは検索フォームが存在する任意のページへのパス

    # placeholderが「検索」である入力フィールドを探して値を入力
    fill_in(placeholder: '検索', with: '検索したいテキスト')

    # fill_inで指定したフィールドを再度探し、Enterキーを送信
    find('input[placeholder="検索"]').send_keys(:return)

    # 期待する結果（例：特定のテキストがページに表示されること）を確認
    expect(page).to have_content('期待する検索結果のテキスト')
  end
end
```

### ページの見出し等を調べたいときはシンプルに。
もし、画面トップに表示されるh1タグの内容が特定の文言と一致するかどうかのチェックをしたいだけであれば、have_contentマッチャを使用しなくても、以下のようにしてもいい気がする。

```
it 'ページ見出しがHOGEであること' do
  h1_text = page.find('h1').text
  expect(h1_text).to eq('HOGE')
end
```
