## Controller単位でレイアウトを指定する

Railsのプロジェクトで新しいページを作成したのだが、CSSやJSなどのアセットファイルがまるで反映されないことがあった。

基本的にRailsではapplication.html系統を読み込むようなイメージだったけど、よくよくみたら参照しているlayoutがちがった。

`layouts: <layoutのファイル名>`で読み込むlayoutを選択できる。

また、layoutのファイル名の部分でprivateメソッドを呼び出すようにすれば、コントローラ内のアクションに応じてlayoutのだし分けも可能。
