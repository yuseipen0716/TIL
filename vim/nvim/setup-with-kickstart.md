## Neovimセットアップ

### キーバインドメモ
キーバインド| 内容
--- | ---
`<leader>sf `| ファイル名のあいまい検索
`<leader>sr` | ファイル名のあいまい検索（前回の検索の続きから）
`<leader>?` | 最近開いたファイルのリストを出して、開くことができる


### Tips
#### yankの範囲指定
今の行から123行目までをyankしたい！というとき
`:.,123y` （コマンド入力）

yのところをdにしたら削除になるよ。

#### コメントアウトのしかた
`numToStr/Comment`というプラグインが入っていた。

ひとまず、`gcc`でcurrent line のコメントアウトができる。

---

参考リンクmemo
- https://github.com/williamboman/mason.nvim/issues/1493 (solargraph 詰まったときのメモ)
