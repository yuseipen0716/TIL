## html-to-imageでHTMLを画像化

ページのスナップショットを撮りつつ、pdf化もしたかったため、html-to-image（[bubkoo/html-to-image](https://github.com/bubkoo/html-to-image)）というライブラリを使用した。

はじめはhtml2canvas + jsPDFを使用してみたのだが、html2canvasのレンダリングがうまくいかないのか、CSSで調整した部分がずれまくったり、textarea内の改行が無効になってしまったり、
苦戦した。

html-to-imageというライブラリもあったため、試してみた。

今回やりたかったこと
- HTML → 画像 → PDF化
- inputタグに対するsubmitのボタンなどは消して出力したい

html2canvasではignoreしたい要素を指定することができたので、こちらでもできるはず。documentを読んでいると、filterというoptionがあった

> ### filter
> `(domNode: HTMLElement) => boolean`
> A function taking DOM node as argument. Should return true if passed node should be included in the output. Excluding node means excluding it's children as well.
> You can add filter to every image function. For example,
> ```javascript
> const filter = (node)=>{
> const exclusionClasses = ['remove-me', 'secret-div'];
> return !exclusionClasses.some(classname=>node.classList.includes(classname));
> }
>
> htmlToImage.toJpeg(node, { quality: 0.95, filter: filter});
> ```
> or
> ```javascript
> htmlToImage.toPng(node, {filter:filter})
> ```
> Not called on the root node.

こちらを見て試してみたが、思うようにいかず、こちら( [Exclude element when download with dom-to-image library](Exclude element when download with dom-to-image library) )
の質問の回答を参考に修正。


```javascrip
import React, { useCallback, useRef } from 'react';
import { toPng } from 'html-to-image';

const App: React.FC = () => {
  const ref = useRef<HTMLDivElement>(null)

  const onButtonClick = useCallback(() => {
    if (ref.current === null) {
      return
    }
    // 別コンポーネント内でignoreしたい要素に'ignore-me'というidを設定
    const ignoreNode = document.getElementById('ignore-me')

    toPng(ref.current, {
      cacheBust: true,
      filter: (node) => node !== ignoreNode,
      })
      .then((dataUrl) => {
        const link = document.createElement('a')
        link.download = 'my-image-name.png'
        link.href = dataUrl
        link.click()
      })
      .catch((err) => {
        console.log(err)
      })
  }, [ref])

  return (
    <>
      <div ref={ref}>
      {/* DOM nodes you want to convert to PNG */}
      </div>
      <button onClick={onButtonClick}>Click me</button>
    </>
  )
}
```
これで除外したい要素は非表示にして画像化することができた。
