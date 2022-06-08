# Reduxチュートリアルいろいろ

Reduxチュートリアルをやる中で学んだことなどを書き残していく。

あとでドキュメントを読み直すのが大変なので、要所は翻訳して残しておく。

## Tutorial

ここに学んだことをメモしていく。

SUMMARY(Tutorial)

- `configureStore`でRedux storeをつくるよ
  - `configureStore`はreducer関数を名前付き引数として受け取るよ
  - `configureStore`は自動でstoreをいい感じにセッティングしてくれるよ
- ReactアプリケーションにRedux storeを提供するよ
  - React-Reduxの<Provider>コンポーネントで<App />を囲うよ
  - `<Provider store={store}>というふうに、storeをpropsとしてRedux storeに渡すよ
- `createSlice`でslice reducerをつくるよ
  - name(string)とstateの初期値、名付けたreducer関数を添えて`createSlice`を呼び出すよ。
  - `Immer`ライブラリを使用していて、stateの更新はreducer関数内でmutateに行われるかもよ
  - 生成されたslice reducerとaction creatorsをexportするよ
- Reactコンポーネント内でReact-Reduxの`useSelector`, `useDispatch`hooksを使うよ
  - storeからデータを読み取るには`useSelector`hookを使うよ
  - `useDispatch`hookでdispatch関数を取得して、必要に応じてactionをdispatchするよ(割り当てる?的な?)

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
  
`src/features/counter/counterSlice.js`がなければ作成する。その中に`createSlice`APIをRTKからimportする。
  
sliceを作成するにあたって、文字列でnameとstateの初期値を定義してあげる。そんで、その中で、どのようにstateが更新されるのかを定義したreducer関数を書く。
  
Reduxではstateの更新はイミュータブルな方法で行うように記述する必要があるが、RTKの`createSlice`と`createReducer`のおかげで、mutatingな書き方で書いても大丈夫になる。
  
#### Add Slice Reducers to the Store
 
`app/store.js`に戻って、先ほど作成した


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



