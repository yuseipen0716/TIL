## VSCodeでスニペットを設定する方法

VSCodeは日本語化してあります。

1. 歯車マーククリック
2. コマンドパレット
3. snippetsと打つと「基本設定: ユーザースニペットの構成」というのが出てくるので、クリック
4. スニペットを設定したい言語等を選択（今回はerbを選択)
5. ``` json
   {
	// Place your snippets for erb here. Each snippet is defined under a snippet name and has a prefix, body and 
	// description. The prefix is what is used to trigger the snippet and the body will be expanded and inserted. Possible variables are:
	// $1, $2 for tab stops, $0 for the final cursor position, and ${1:label}, ${2:another} for placeholders. Placeholders with the 
	// same ids are connected.
	// Example:
	// "Print to console": {
	// 	"prefix": "log",
	// 	"body": [
	// 		"console.log('$1');",
	// 		"$2"
	// 	],
	// 	"description": "Log output to console"
	// }
   ```
   のように出てくるので、例に従って{}内に追加入力していく
6. `Print to console`の部分はスニペットの簡単な説明文。`prefix`の部分は実際に入力する文字列を。`body`の部分には生成して欲しいコードを。`$0`でカーソル位置を指定。`description`のところは一旦そのままで
7. 続けてスニペットを登録する場合には`,`で区切る

### 例
``` json
{
	"put <%=%>": {
		"prefix": "pe",
		"body": [
			"<%= $0 %>"
		],
		"description": "Log output to console"
	},
	"put <%%>": {
		"prefix": "er",
		"body": [
			"<% $0 %>"
		],
		"description": "Log output to console"
	},
	"put <%#%>": {
		"prefix": "pc",
		"body": [
			"<%# $0 %>"
		],
		"description": "Log output to console"
	},
	"put html_tag": {
		"prefix": "html",
		"body": [
			"<html>$0\n</html>"
		],
		"description": "Log output to console"
	},
	"put body_tag": {
		"prefix": "body",
		"body": [
			"<body>$0\n</body>"
		],
		"description": "Log output to console"
	},
	"put div_tag": {
		"prefix": "div",
		"body": [
			"<div $0>\n</div>"
		],
		"description": "Log output to console"
	},
	"put p_tag": {
		"prefix": "p",
		"body": [
			"<p>$0</p>"
		],
		"description": "Log output to console"
	}
}
```

上記したように、`\n`を使えば改行も挟める

