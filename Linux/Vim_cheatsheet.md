## Vimまとめ
兎にも角にも、`vim --version`でvimがインストールされているのかを確認。

ちゃんとインストールされていれば、version情報が出力される。インストールされていないのであれば、not found的なエラーメッセージが出る。

そうなったら、`sudo apt install vim`でinstall　(for ubuntu)

### Vimの基本的なコマンド

コマンド | 内容
---|---
:q | 終了
:w | 保存
:q! | 保存せずに終了
h | ←
j | ↓
i | ↑
l | →
x | カーソル位置にある文字を削除
i | カーソルの左側に文字を入力(insertモードに移行) 
a | カーソルの右側に文字を入力(insertモードに移行)
Esc | insertモードを離脱。ノーマルモードへ。
w | 前方に単語ひとつ分移動
b | 後方に単語ひとつ分移動
W | スペース区切りで前方に単語ひとつ分移動
B | スペース区切りで後方に単語ひとつ分移動
0 | 行頭へ移動
$ | 行末へ移動
gg | 1行目に移動
G | 最後の行に移動
<数字>G | <数字>行目に移動
d$ | 行末までをデリート(カット)
d0 | 行頭までをデリート(カット)
x, dl | 1文字をデリート(カット)
dw | 単語ひとつをデリート(カット)
dgg | 最初の行までをデリート(カット)
dG | 最後の行までをデリート(カット)
p | プット(ペースト)
y$ | 行末までをヤンク(コピー)
y0 | 行頭までをヤンク(コピー)
yl | 1文字をヤンク(コピー)
yw | 単語ひとつをヤンク(コピー)
ygg | 最初の行までをヤンク(コピー)
yG | 最後の行までをヤンク(コピー)
dd | 現在カーソルがある行をデリート(カット)
yy | 現在カーソルがある行をヤンク(コピー)
J | 行を連結(xコマンドで行をまたいで文字を削除できるようにするため）
u | アンドゥ(undo)
Ctrl + r | リドゥ(redo)
/<文字列> | 下方向に向かって<文字列>を検索する。
?<文字列> | 上方向に向かって<文字列>を検索する。
n | 次の検索結果に移動
N | 前の検索結果に移動
%s/<置換元文字列>/<置換後文字列>/g | <置換元文字列>を<置換後文字列>に置換




