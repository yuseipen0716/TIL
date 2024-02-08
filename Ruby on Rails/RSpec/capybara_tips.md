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
