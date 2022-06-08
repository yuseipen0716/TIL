# Reduxチュートリアルいろいろ

Reduxチュートリアルをやる中で学んだことなどを書き残していく。

あとでドキュメントを読み直すのが大変なので、要所は翻訳して残しておく。

## Tutorial

ここに学んだことをメモしていく。

## Documentメモ

ここはTutorial周辺のドキュメントの簡単な訳をメモしていく。

### Redux Toolkit Quick Start(Tutorial)

ひとまずinstall 

```
yarn create react-app my-app --template redux
```

プロジェクト作成後、my-appディレクトリ内に移動

#### Create a Redux Store

`src/app/store.js`というファイルを作成(すでにあった)して、`configureStore`APIをReduxToolkitからimportしてくる。

これでReduxのstoreが作成され、Redux DevToolsの拡張機能も自動的に設定される。

#### Provide the Redux Store to React

storeの作成後、React-Reduxの<Provider>コンポーネントで<App />コンポーネントをラップする。
  
先ほど作成したRedux storeをimportして、Providerコンポーネントのpropsとしてstoreを渡す。

#### Create a Redux State Slice


### (Redux Essentials)Introduction

ここで学べる事

- Redux とは何か、また、どうしてReduxを使うのか
- Reduxのキーとなる用語やコンセプトについて
- Reduxアプリ内のデータフローはどんな感じか

学習要件(このくらいの知識はある前提で説明していくよ)
- HTML/CSS
- ES6の機能、文法
- ReactにおけるJSX記法、Stateや関数コンポーネント、Props、Hooksの知識
- JavaScriptの非同期処理、AJAXrequestの作成

(Hooksについての知識、JavaScriptの非同期処理のところがふんわり理解なのはいけない気がするので、同時並行で頑張る。)



