## CSS 疑似要素 nth-childとnth-of-typeについて

```html
<ul>
  <li>a</li>
  <li>b</li>
  <li>c</li>
  <li>d</li>
</ul>
```

というようなリストがあるとき、3つ目の`c`の部分だけにCSSを当てたいなんて言うときがあるかもしれない。

そんな時に使用するのが、`:nth-child()`という疑似要素。

この場合、

```css
ul li:nth-child(3) {
  font-weight: bold;
}
```
というようにしてあげると、`c`だけ太字になるといった感じ。

ただのliタグが並んでいるだけならいいが、この中にdivタグが混じったり、何かほかの要素が混じってしまうと、`:nth-child()`はうまく動作しない。

（`:nth-child()`は要素のまとまりの上から何番目か、というように指定した要素を判断するらしい。）

そんな時は`nth-of-type()`という疑似要素を使用するとよい。

`:nth-of-type()`は指定した要素（この場合はliタグ）の何番目か、という判断で動作する。

```html
<ul>
  <li>a</li>
  <div><p>あいうえお</p></div>
  <li>b</li>
  <li>c</li>
  <li>d</li>
</ul>
```
こういう構造はほとんどないとは思うが、あくまで例として。この場合に`c`の文字を太字にするなら

```css
ul li:nth-of-type(3) {
  font-weight: bold;
}
```

としてあげればいいよ、というお話でした。


参考: [:nth-child と nth-of-type の違い](https://planbworks.net/nth-child_nth-of-type.html)

