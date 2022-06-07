## Documentざっとまとめ

[公式ドキュメント](https://redux.js.org/introduction/getting-started)サラ読みだと、ものすごい勢いで内容が抜けそうなので、ふんわり日本語訳しながらかみ砕いて読んでいく。

## Introduction

Reduxは一貫した形で動作し、client,server,nativeのような異なった環境でも実行でき、テストも簡単にできるアプリケーションを作ることを手助けする。
Reduxは素晴らしい開発体験を提供してくれる。

ReduxはReactと一緒に使うことができる。ほかのライブラリでも使える。2kbほどの依存性というほど、ちっこいけど、大きなエコシステムを持っている。

### Instllation

ここは、ずべこべ言わず、公式がおすすめしてくれている方法で、tool_kit等をinstallしたり、create-appしましょう

### Basic Example

アプリケーションのglobalなstate全体は、1つのストア内のオブジェクトツリーに格納される。そのstateを変更する、ただ一つの方法はactionを作成すること。あるオブジェクトに何が
起こるかを記載し、それをstoreに命令する(dispatchってどう訳すのが正解なんやろ)。
アクションに応じてstateを更新する方法を指定するには、純粋なreducer関数をかく。このreducer関数っていうのは、古いstateを元に新しいstateを算出する関数。

stateを直接書き換えるのではなく、actionと呼ばれるplainなオブジェクトで、state変異の方法を指定してあげる。
そんで、reducerと呼ばれる、アプリケーションのstate全体をどのように書き換えるか決めるための、特殊な関数を書く。

典型的なReduxアプリの場合、1つのroot_reducer関数をもつstoreが一つだけある。アプリが大きくなるにつれて、ルートreducerを小さなreducerに分割し、
stateツリーの異なる部分に独立した形で動作させるようになる。
これはまさに、Reactアプリはただ一つのルートcomponentしかないが、その中で小さなcomponentがたくさんあるような感じに似ている。

このアーキテクチャは、小さなアプリでは大変だと感じるかもしれないが、この美しさ(素晴らしさ)は、大きく複雑なアプリケーションに対応するための拡張性である。
また、すべての状態変化に対してそれがどのように起こったのかを追跡できるため、とっても強力な開発ツールとなりうる。→ユーザセッションを保存してリプレイするだけでOK

### Redux Toolkit

ReduxToolkitを使えば、Reduxの処理の記述やstoreの設定をシンプルにかける。そのおかげで、同じReduxの動作であってもロジックを短くかけたり、読みやすい形で書くことができる。

## Why Redux Toolkit is How to Use Redux Today

Redux Toolkit(RTK)は公式もおすすめするReduxのロジックを書くための手段。`@reduxjs/toolkit`パッケージはcoreの`redux`パッケージをwrapしており、 Reduxアプリケーションを
作る際に必須と思われるAPIメソッドや一般的な依存関係を含んでいる。
RTKはシンプルで、多くのタスクをさばけて、凡ミスをしにくいようなベストプラクティスを提供してくれて、また、Reduxアプリケーションを簡単につくることができる。

RTKはstoreの設定やreducer関数の作成、イミュータブルな状態更新のロジック、stateの切り分けなどの多くのユースケースをシンプルにするユーティリティを兼ね備えている。

### How Redux Toolkit Is Differrent Than the Redux Core

Reduxって何?

Reduxとはまさに、

- グローバルstateを含む、一つのstore
- アプリに何か変化があった時、storeに対してプレーンオブジェクトのアクションをdispatchする
- これらのアクションを検知して、イミュータブルなstateの更新を返す、純粋なreducer関数






