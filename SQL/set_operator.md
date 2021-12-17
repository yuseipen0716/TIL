## 集合演算子のこと

### 和集合　UNION

2つのSELECT文をUNIONで繋いで記述すると、２つの和集合が返される

書式

```
SELECT 文1
UNION (ALL)
SELECT 文2
```

#### 集合演算子を使う際のポイント

- 選択列リストの列数とそれぞれのデータ型が一致していないといけない
- 集合演算子でORDER BYを使う際には最後のSELECT文に記述する
- 列番号以外による指定（列名やASによる別名)の場合は、1つ目のSELECT文のものを指定する

### 差集合　EXCEPT/MINUS

あるSELECT文と別のSELECT文を比較して、内容が重複するものを差し引いた集合

```
SELECT 文1
EXCEPT (ALL)
SELECT 文2
```

差集合を求める際には、どちらのSELECT文を基準にするかが重要。1-2と2-1の答えが異なるように、基準となるSELECT文が変わると、結果も変わってきてしまう。

### 積集合 INTERSECT

2つのSELECT文で共通するところを引っ張ってくる。

```
SELECT 列名... FROM テーブル名
INTERSECT (ALL)
SELECT 列名... FROM テーブル名


