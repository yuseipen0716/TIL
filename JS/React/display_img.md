## 画像を読み込み表示する

Reactで画像を表示するコンポーネントを作りたい場面があったが、JSXの中身のimgタグ内にsrcべた書きするだけでは表示されなかったので、
ここにメモを残しておく。

### importお忘れなく

はじめ、
```javascript
import React from 'react';

export const Image = () => {
  return(
    <>
      <img src="../resume.png" alt="resume" />
    </>
  )
};
```
このような書き方をしていたが、imageが読み込まれず、altのテキストが表示されていた。調べてみると、画像ファイルもimportしてあげれば表示されるらしい。

以下のように修正した。画像ファイルをimportし、srcの中にモジュール名を記載してあげる。
```javascript
import React from 'react';
import resume from '../resume.png'

export const Image = () => {
  return(
    <>
      <img src={resume} alt="resume" />
    </>
  )
};
```
