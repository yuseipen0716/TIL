## vimにpluginを導入

vimにプラグインを入れてみたので、その時の記録と、参考にさせていただいたページのメモ

### dein.vim

今回は、dein.vimというプラグインを管理してくれるプラグインを導入した。

[こちら](https://note.com/noabou/n/nfbfc099c0361)の記事を参考にさせていただきました。

このプラグインを使うと、`~/.vim/dein.toml`というファイルに追加したいプラグインを書き足すことでプラグインを導入できる。

```
# Required:
[[plugin]]
repo = '<github_name>/<repository_name>

```
というようにプラグインを書いて追加していく。

新しいプラグインを追加したいときには、この下に`[[plugins]]...`と続けて書く。

### プラグインとは違うけど

プラグインとは違うけど、`~/.vimrc`に書いておくおすすめ設定も残しておく

```
inoremap <silent> jj <ESC>
```
これによって`jj`と打つだけでinsertモードを抜けてnormalモードに切り替えることができる。Escキーって地味に遠いからね。
 
 ### プラグインをアンインストールしたいとき
 
 tomlファイルからプラグインの記述を消して、以下を順に実行。
 
```
:call map(dein#check_clean(), "delete(v:val, 'rf')")
```

```
:call dein#recache_runtimepath()
```

もし、更新等を自動化したいのであれば

```
let g:dein#auto_recache = 1
```
をinit.vimに追加。
