## データベース設計あれこれ

### データベース構築の流れ

1. 要件
2. データベース設計作業
     1. 概念設計
     2. 論理設計
     3. 物理設計
3. DDL

---

### 概念設計

- 管理すべき情報はどのようなものか
- 要件に登場する情報をざっくり整理
- データベースやシステムに関する考えは一旦おいておく

要件を実現するために、どのような「情報の塊」を管理しなければならないかを明らかにする。

この「情報の塊」のことをエンティティという。エンティティは複数の属性(attribute)を持っている。エンティティ同士にどのような関連があるかも明らかにする。

エンティティ同士の関連をER図(entity-relation diagram)に表す。たくさん書いて慣れましょう。

エンティティ、属性、関連の書き方など調べる。

---

### 論理設計

- 概念設計で明らかになった情報を、RDBを使用する前提で構造を整理し、具体化
- どのようなテーブルを作り、どのような列を作るか、などを明らかにする
- 型や制約などはここでは一旦おいておく

ここで出てくるのが正規化(normalization)

正規化とは、矛盾したデータを格納できないようにテーブルを複数に分割していく作業

整合性が崩れにくい優れたテーブルというのは、「1つの事実は1箇所に(one-fact in one-place)」という原則に従ったもの。正規化の手順に則って、このようなテーブルを作成していく。

テーブルがどの程度正規化されているかの度合いで第1正規形から第5正規形まで存在するが、通常のシステムにおいては第3正規形まで正規化されていれば問題ない。


#### ■第1正規形が目指す形

- テーブルの全ての行の全ての列に1つずつ値が入っている状態。
- 繰り返しやセルの結合がない

#### ■第1正規化の手順

1. 繰り返しが起こっている列を別テーブルに切り出す
2. 切り出したテーブルにおける仮の主キーを決める
3. 切り出し元のテーブルの主キーを切り出し後のテーブルに貼り付けて先ほど設定した仮の主キーと合わせて、複合主キーとする

#### ■第2正規形が目指す形

複合主キーを持つテーブルの場合、非キー列は全て、複合主キーに関数従属する。（複合主キーの一部に関数従属する部分関数従属の状態の非キー列があってはいけない）

そもそも**関数従属**とは、「列Aのデータがわかれば列Bのデータも特定できる」という関係。この場合、BはAに関数従属しているという。

複合主キーのうち、どちらかにだけ関数従属しているような関係のことを**部分関数従属**という

#### ■第2正規化の手順

1.  部分関数従属している列を別テーブルに切り出す。
2.  部分関数従属先となっていた列を先ほど切り出したテーブルにコピーして主キーとする

#### ■第3正規形が目指す形

テーブルの非キー列は主キーに直接関数従属する。

非キー列において、主キーに間接的に関数従属しているケースがある。たとえば、主キーに関数従属している「利用者ID」列があるとして、それに対して「利用者名」という列があるとすると
「利用者名」は「利用者ID」に関数従属し、「利用者ID」は主キーに関数従属するという、間接的な関数従属関係になっている。このような関係のことを**推移関数従属**という

#### ■第3正規化の手順

1.  推移関数従属している列を別テーブルに切り出す。
2.  推移関数従属先となっていた列を先ほど切り出したテーブルにコピーして主キーとする


#### 正規化まとめ

```
正規化とは、矛盾したデータを格納できないようにテーブルを複数に分割していく作業

・第1正規化　　繰り返し部分をなくす
・第2正規化　　部分関数従属をなくす
・第3正規化　　推移関数従属をなくす

それぞれの段階でなくしたいものは別テーブルに切り出して、キーを引っ張ってくる
```

正規化によってデータの整合性は守られる一方、データの取得の際にはテーブル結合を行わなければならないなど、パフォーマンスを低下させる一因となってしまう。

しかし、正規化しないことによって、更新の際にデータの齟齬が生まれたり、デメリットも多い。基本的には正規化はすべき(第3正規形までは)






---

### 物理設計

- 特定のRDBMSを使用することを前提に、論理設計で明らかになった情報をさらに具体化
- テーブルや列について、型や制約、インデックス、デフォルト値などの細かな要素を確定していく
