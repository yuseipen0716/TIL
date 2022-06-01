## 遭遇したerrorたち

### npx create-react-app できない

Reactチュートリアルをlocalで環境構築して進めてみようと思ったが、そもそも最初のコマンドでコケた。

``` javascript
PS C:\Users\user_name> npm install
npm ERR! code ENOENT
npm ERR! syscall open
npm ERR! path C:\Users\user_name/package.json
npm ERR! errno -4058
npm ERR! enoent ENOENT: no such file or directory, open 'C:\Users\user_name\package.json'
npm ERR! enoent This is related to npm not being able to find a file.
npm ERR! enoent
```

とりあえずpackage.jsonがないと。

`npm init`でpackage.jsonを作成。その後`npx create-react-app <app_name>`するも、timeoutのエラーが出てしまった。

ちょっといろいろ試したけど、うまくいかないのでNode.jsのパッケージ管理ツールからインストールしなおすことに。

前まで使おうとしていたnというパッケージ管理ツールはwindowsでは使えないみたい。nvm-windowsを使用することに。

基本的には下記リンクの記事を参考にさせていただきました。Macでやるときはもうちょっと簡単なんだろうか。

> 参考: [React.jsの環境構築(windows編)](https://zenn.dev/kagetugu/articles/eec07c364f9153)

nvmで推奨バージョンのNode.jsを入れて、npm,yarnをinstall

```
yarn create react-app <app_name>
```

めちゃくちゃ時間かかったけど、エラー等でずに完了

```
$ cd <app_name>

$ yarn start
```

で

![image](https://user-images.githubusercontent.com/81737622/171358145-4ec69cd0-46b3-44cf-bc0a-32ce54866ffc.png)

できた。







