## findコマンドを使う際に注意すべきこと

findコマンドで　-nameや-inameなどで検索条件を指定する際に、`*.txt`というようにパス名展開を使用して検索条件を指定したい場合もある。

そのときに、`find . -name *.txt -print`とするとエラーがでます。

どうしてか。

上記のように検索条件を指定すると、コマンドを受け付けた時点で、`*`の部分のパス名展開が行われてしまい、カレントディレクトリにtest1.txt, test2.txtの二つのファイルがあるとすると、Kernelさん的には

`find . -name test1.txt test2.txt -print` と入力されたと判断してしまう。

これを防ぐには`*`や`?`にクオート、ダブルクオートをつけてあげれば良い。

`find . -name '*.txt' -print`　としてあげる。

こうすることで、Kernelさんにコマンドを渡したときはパス名展開されていない状態で渡され、条件を`*.txt`の状態で検索することができるみたい。

これがどこかで必要になるかはわからないけれど、最初に打ったみたいなコマンドを入れてエラーが出たときに「絶対合ってるはずなのに！」って詰まる未来が見えたので、覚書として残しておく。
