## トランザクション

### トランザクションとは

AさんからBさんへ1000円送金するという場合、Aさんの残高を-1000、Bさんの残高を+1000というUPDATE文が二つ発生することになる。

このUPDATE文がなんらかの原因で中断されてしまったとする。Aさんの残高が-1000されたけど、Bさんの残高が増えていない場合、大変。

このようなトラブルは絶対に起きてはいけないので、上の二つのSQLは一つのかたまりとしておく必要がある。そうすると、二つのUPDATEが無事成功すれば、処理完了、もし何らかの原因で処理が中断された場合、
SQL文の片方は完了…ではなく、どちらのSQL文も実行していない状態（まっさらな状態）となっている必要がある。

このように、1つ以上のSQL文をひとかたまりとして扱うように指示ができる。このかたまりのことを**トランザクション**という。

### トランザクションの制御

DBMSはトランザクションを

- 途中で処理が中断されないようにする
- 途中で他の人の処理が割り込めないようにする

のように扱っている。（トランザクションの制御)

トランザクションの処理において、トランザクション内の単品のSQLが実行されると、それぞれ「仮に書き換えた」状態になる。全てのSQL文が問題なく実行できたときに初めて「仮に書き換えた状態」から「処理が確定した」状態になる。
このことを**コミット**という。

途中でなんらかの問題が起き、まっさらな状態に戻すこことを**ロールバック**という。

---

### トランザクションを使うための指示

指示 | 意味
--- | ---
BEGIN | 開始の指示。この指示以降のSQL文を一つのトランザクションとする
COMMIT | 終了の指示。この指示までを一つのトランザクションとし、変更を確定する
ROLLBACK | 終了の指示。この指示までを一つのトランザクションとし、変更の取り消しをする。

例

``` SQL
BEGIN; -- これより下がトランザクション
-- アーカイブテーブルへコピーする処理
INSERT INTO アーカイブ
SELECT FROM 現行データ WHERE句;
-- 現行データから、アーカイブに移したデータを削除
DELETE FROM 現行データ WHERE句;
COMMIT; -- ここより上がトランザクション
```
　
 こうすると、`BEGIN;`より下、`COMMIT;`より上がトランザクションとなり、不可分なものとして扱われる。この、トランザクションが不可分のものとして扱われることを**トランザクションの原子性**という。

---

### トランザクションの分離

あるデータベースの同じ行に複数のSQL文が飛ばされることがある。

Aさんが銀行口座(残高30万円)から５万円を引き出す（トランザクション1)処理を行うのと同じようなタイミングで、口座から家賃の引き落とし(6万円)（トランザクション2）があったとする。

1. 残高30万円から5万円引く(残高25万円（仮））　←トランザクション1
1. 家賃引き落とし6万円(残高19万円(仮））　←トランザクション2
1. トランザクション1　コミット　(残高25万円）
1. トランザクション2　コミット(残高19万円）

という処理の流れになると考える。この時になんらかの原因でトランザクション1が中断され、ロールバックしたとする。トランザクション2は仮確定された残高25万円から6万円引き落としており、この場合、Aさんは銀行から
5万円引き出せなかったにもかかわらず、最終的に家賃を払い終えて残った残高は19万円になってしまう！！

こうなってしまうと大問題なので、トランザクション1、トランザクション2はそれぞれ分離して、トランザクション1を行なっている間、トランザクション2には待ってもらう必要がある。そうすれば、仮にトランザクション1が失敗してロールバック
しても、トランザクション2はトランザクション1が実行される前の段階でのデータを取得して処理することができるようになる。

DBMSはこのように、個々のトランザクションの分離性(isolation)を保つために`あるトランザクションを実行する際に他のトランザクションから影響を受けないよう、分離して実行する`という制御をしている。

その際、DBMS内部では、トランザクション1を実行中は実行している行にロックをかけ、他の処理からは読み書きできないようにしている。つまり、トランザクション1実行中に同じ行を処理したいトランザクション2があると、そちらには
お待ちいただくことになる。トランザクション1がコミットまたはロールバックすると、そのロックは解除され、トランザクション2で読み書きができるようになる。

DBMSに対して複数の処理を同時要求することによる副作用3つ　↓

#### ダーティーリード(dirty read)

コミット前の仮確定の状態の変更を、他の要求の時点で読めてしまうという副作用

#### 反復不能読み取り(non-repeatable read)

ある二つのSELCT文を実行する際、その間でUPDATEが挟まれると、二つのSELECT文で検索結果に差異が生じてしまう副作用。

当然と言えば当然だが、それぞれの統計の整合性が保たれなくなってしまうため、NG

#### ファントムリード(phantom read)

ある二つのSELECT文の間にINSERTが挟まれ、1回目と2回目のSELECT文で結果の行数が変わってしまう副作用。2回目のSELECT文が1回目のSELECT文で取得した行数に依存した処理の場合、困ってしまう。

---

### トランザクション分離レベル

たくさん、ロックがかかると、待ち時間も増える。だからといって、ロックをかけずに処理を行うと、上記した副作用を生じてしまうため、よくない。

そこで、どの程度厳密にトランザクションを分離するのかを**トランザクション分離レベル(transaction isolation level)**として指定することができる。

分離レベル | dirty read | non-repeatable read | phantom read
--- | --- | --- | ---
READ UNCOMMITTED | 恐れあり | 恐れあり | 恐れあり
READ COMMITTED | 発生しない | 恐れあり | 恐れあり
REPEATABLE READ | 発生しない | 発生しない | 恐れあり
SERIALIZABLE | 発生しない | 発生しない | 発生しない

上に行くほどロックはキツくないので高速だが、危険性あり。下に行くほど、低速になるが安全

多くのDBMSではREAD COMMITTEDがデフォルト

#### 明示的に分離レベルを指定

``` SQL
トランザクション分離レベルの指定

SET TRANSACTION ISOLATION LEVEL 分離レベル名
または
SET CURRENT ISOLATION 分離レベル名
```






 
 
 
