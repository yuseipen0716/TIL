## Vim(vi)おぼえがき
 
terminal上で` vi .zshrc` のように打つとVimが立ち上がる。

どんな風に操作すればいいか、よく忘れるので書き残しておく。

### terminalでの操作

`vi ファイル名` Vim（コマンドモード）が立ち上がる

### viコマンドモードでの操作

 #### 保存とか終了とか  
`:q`　終了  
`:qa!`　保存せず終了  
`:wq` ファイルに保存して終了  
`:help`　ヘルプを表示  

#### モード変更
`i`　インサートモードへ移行（カーソルの右から）  
` a `　インサートモードへ移行  

他にもIとかAとかもあるみたいだけど、また今度。


### インサートモード中の操作
`Escキー`インサートモードを終了して、コマンドモードにもどる。　　

### おおまかな操作の流れ　　
1. terminal上で`vi ファイル名`  
1. `a` または`i`　でインサートモードへ。  
1. 入力したり、消したり
1. インサートモードを終了するために`Escキー`を押す
1. コマンドモードに帰ってきたので、変更を保存して終了する場合には`:wq`を入力すればOK
