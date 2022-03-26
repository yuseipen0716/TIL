## layoutを使用してみる

topページではこういう表示をしたい、だけどshowのページではこの部分のヘッダーは表示したくない、などapplication.htmlを使っているだけでは難しい表示があるかもしれない。

例えばtop#indexで個別のレイアウトを使用したい時は

- まず、app/vies/layouts配下にlayoutテンプレートを作成(top.html.hamlという名前と仮定)
- app/controllers/top_controller.rbを以下のように変更
  ``` ruby
  def index
    render layout: "top"
  end
  ```
