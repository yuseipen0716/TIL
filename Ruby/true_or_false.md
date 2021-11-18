## Rubyの真偽値について


- false,nilは偽
- それ以外は真

というのがRubyにおける真偽値の考え方。

``` ruby
def comp(a, b)
  if a == b
    return true
  else
    return false
  end
end
```
こんな風に書かなくても
``` ruby
def comp(a, b)
  a == b
end
```
これだけでtrue or falseは返ってくる。

また、このメソッドの戻り値を後の条件分岐等で使う時も

` if comp(a, b) == true `

のように書かなくても

` if comp(a, b) `

で十分なんだね。

whileb文で例えば、「０が入力されたら繰り返し終了、1が入力されたら繰り返し」という風な繰り返し処理をしたい時

``` ruby
while true
# 処理内容は割愛
break if switch == 0
break if switch != 0 && switch != 1
end
```
と書いてあげれば、0が入力された、また0でも1でもない値が入力された時は処理終了/1が入力されれば繰り返し

というwhile文が書けるってワケ

ただし、適切にbreakの条件を入れてあげないと簡単に無限ループに陥るので注意。


