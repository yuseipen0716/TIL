## JSメモ

### indexOfメソッド

`String.indexOf('foo')'とすると、Stringの中でfooを含む最初の文字列が見つかったインデックスを返す。なければ`-1`が返ってくる。

内部サイト・外部サイトを判定するロジックで使用されていた。`<調査したいリンクURL>.indexOf('hosting_url')`として-1が返ってきていれば外部サイトと判定

少し似た感じで正規表現を使用して調査したいときにはsearchメソッドを使用した。

`String.search(/\d{4}-d{3}/)`みたいに使う。Stringの中に、パターンにマッチするものがなければ`-1`が返ってくる。

サイト内の特定のページを結果から弾きたいときなどに使用した。

### 論理演算子

`&&` → 左辺がtrueなら右辺を出力

if文の中の条件で「かつ」の意味で使用することが多かったが、出力の出し分けをワンラインで記述する際に

```
<条件A> && <出力B>
```

とすると、条件Aがtrueなら出力Bを出力するというような意味合いになる。これが「かつ」の意味。

```
if (a && b)
```
aがtrueならbのtrue or falseの出力をみている。


また`||`は、「または」という意味でよく使われる。

```
<条件A> || <出力B>
```

`||`のときは条件Aがfalseの場合に出力Bを出力する。そのため、「または」の意味で使用できる。

### 文字列を数値に
```javascript
str = "123"
int =  +str
```

文字列のまま加減算等は行えるようだが、念のため、明示的に数値に戻して計算してあげた方が予期せぬ事態に陥らないだろう。

わかりにくいので、この書き方を嫌う人もいるみたい。Number()の方が親切か。

### 真偽値
JavaScriptだと数字の0がfalseという扱いになる。そのため、配列型の存在チェックで`if (array.length !== 0)`みたいな条件を見かけるのかな

### 関数宣言と関数式 巻き上げについて
以下のようにfunctionキーワードの横に関数名を書き、その後ろに関数の本体を記述することを関数宣言という
```javascript
function hoge() {
  console.log('hoge')
}
```

`hoge();`というように関数名に()をつける形で呼び出しができる。

もう一つの関数の定義方法として、関数を変数に代入する形で作成する「関数式」というものがある
```javascript
const hoge = function() {
  console.log('hoge')
}
```

これも`hoge()`で呼び出すことができるが、関数宣言と違う点は巻き上げが起こるか起こらないか。

関数宣言の場合は、関数宣言の前に関数呼び出しをしても、エラーが出ず実行される。

関数式の場合は、関数式の定義前に関数を呼び出すと、エラーがでる。

---

### 関数の戻り値
ごく基礎的な話だけど、Rubyと少し違いがあって最初戸惑ったので、メモ。

```javascript
const hoge = function() {
  console.log('hoge')
}
```
この関数の場合はコンソールに出力をするだけだが、関数内にreturn文を記述することで、関数に戻り値を持たせることができる。

```javascript
const hoge = function() {
  return 'hoge';
};
```
このような関数式を作成した場合、以下のようにすると、関数の戻り値をコンソールに出力できる
```javascript
console.log(hoge());

=> 'hoge'
```

### デストラクチャリング

オブジェクトを変数に代入したり、受け取る際に、必要なプロパティのみ取捨選択できる機能

コードサンプル->Reactハンズオンラーニングのものを引用
```javascript
const sandwich = {
  bread: "dutch crunch",
  meat: "tuna",
  cheese: "swiss",
  toppings: ["lettuce", "tomato", "mustard"]
};

const { bread, meat } sandwich;

console.log(bread, meat); // => dutch crunch tuna
```

Reactで書かれたコードを読んでいるとよく見かける気がする。最初、これなんだ？って躓いたのでメモ

---

### production環境とdevelop環境で表示を変えたい場合
Rails + React + TypeScriptの環境で、本番環境においてはOOの部分は非表示で！みたいな条件があったとき

```javascript
const isProd = () => process.env.NODE_ENV === 'production'
```
としてあげて、
```javascript
{isProd() && (
  <>
    <p>だし分けしたい何か</p>
  </>
)}
```
としてあげればよい。（たぶん）

---








