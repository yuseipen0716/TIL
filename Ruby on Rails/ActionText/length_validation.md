## ActionTextで保存したレコードに文字数制限をつけるのにハマった件

今作っているブログサイトに自動テストを組み込むべく奮闘中。

テキストエディタとして、ActionTextを採用しているんだけど、これがまた色々大変。

テストを書いていたら気づいたのだけど、ActionTextは
リッチなテキストエディタなので、保存される文字列はHTMLタグのお化粧付きで、記事本文のバリデーションを100000文字に設定していても100000文字の入力はできない状況だった。

どうにか100000文字では有効で、100001文字では無効になるようにしたい！と思い、奮闘したので、今後のためにメモを残しておく。


``` ruby
# model
# app/models/article.rb

class Article < ApplicationRecord
  has_many :categorizations, dependent: :destroy
  has_many :categories, through: :categorizations
  has_many :comments, dependent: :destroy

  has_rich_text :body

  validates :title, length: { maximum: 100 }, presence: true
  validates :body, length: { maximum: 100000 }, presence: true

 end

```
モデルはこんな感じ。
テストは↓こんな感じ（一部抜粋)

``` ruby
# spec
# spec/models/article_spec.rb

require 'rails_helper'

RSpec.describe Article, type: :model do

  # タイトルの文字数制限は100字なので、100文字のタイトルは有効であること
  it "is valid with 100-character title" do
    article = Article.new(title: "#{'a' * 100}", body: "test content")
    expect(article).to be_valid
  end

  # タイトルが101文字であるならバリデーションエラーが起きること
  it "is invalid with 101-character title" do
    article = Article.new(title: "#{'a' * 101}")
    article.valid?
    expect(article.errors[:title]).to include("は100文字以内で入力してください")
  end

  # 本文が10万文字なら有効な状態であること
  it "is valid with 100000-character text in body" do
    article = Article.new(title: "test title", body: "a" * 100000)
    expect(article).to be_valid
  end

  # 本文が10万文字より多い場合(100001文字）の場合、バリデーションエラーが起こること
  it "is invalid with 100001-character text in body" do
    article = Article.new(title: "test title", body: "a" * 100001)
    article.valid?
    expect(article.errors[:body]).to include("は100000文字以内で入力してください")
  end
end
```

端的に話すと、タイトルのモデルスペックはパスするけど、本文の方のモデルスペックはパスしない。

調べてみると、初めに話した通り、ActionTextで書かれた文章はHTMLタグや改行文字がついた状態で保存されるため、そのまま文字数のカウントをすると、とんでもなく分量が増えてしまう。

そのため、バリデーションに設定した数値よりも少ない文字しか入力していなくても文字数制限で保存できない不具合が生じていた。

テストの方でbodyのデータを整形する手も考えたが、カスタムバリデーションというものがあると教えてもらったので、それを試すことに。

[公式のドキュメント](https://railsguides.jp/action_text_overview.html)と[こちらの方](https://komaji504.hateblo.jp/entry/2016/05/14/004130)を参考にさせていただき、なんとか実装できた。

最終的なモデルはこんな感じに。

``` ruby
# model
# app/models/article.rb

class Article < ApplicationRecord
  has_many :categorizations, dependent: :destroy
  has_many :categories, through: :categorizations
  has_many :comments, dependent: :destroy

  has_rich_text :body

  validates :title, length: { maximum: 100 }, presence: true
  validates :body, presence: true

  # ActionTextを利用しており、文字数が従来とずれるためカスタムバリデーションを作成
  validate :count_as_plaintext

  private

  def count_as_plaintext
    if body.to_plain_text.gsub(/\n/,"").length > 100000
    errors.add(:body, 'は100000文字以内で入力してください')
  end
 end

```
結局、`body.to_plain_text.gsub(/\n/,"").length`スッピンの文字数を出すことに成功。

`to_plain_text`メソッドはリッチテキストからHTMLタグを剥がすメソッド。ただ、まだ改行文字が残っているため、gsubで消し、文字数を数えた

なかなかActionTextと仲良くなれない。ただ、こういう不具合を見つけられたのもテストコードを書いたからな訳なので、ソフトウェアテストには感謝しかないです。これからも自己研鑽。







