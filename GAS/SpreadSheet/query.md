## スプレッドシート　QUERY関数

`=QUERY(範囲, "select 列A, 集計関数(列B) group by 列A")`

範囲のところは`A:B`としてあげると、A列、B列全体を参照するという意味になる。

`A2:B17`だとセルの範囲をしていするような感じ

`group by`でA列の要素で場合分け（日付ごとの集計を出したりできる）

`label`を使用すると、集計結果の方に見出しをつけられる。

`=QUERY(A:B, "select A, sum(B) group by A label A'日付', sum(B)'計'",1)`

としてあげると、集計結果に「日付」、「計」という見出しがついた。

見出しの下の行が一行空白になっている。これは、列全体を範囲として指定した結果、下の方の空白範囲(null)も集計してしまっているせいみたい。

nullを排除して計算するには`where is not null`をつけてあげる

`=QUERY(A:B, "select A, sum(B) where A is not null group by A label A'日付', sum(B)'計'",1)`

最後についてる`,1`というのは、集計元のデータ（A列、B列のデータ）の1行名は見出し文字ですよ　という意味になる。ここを0にしてあげると、最初の行から集計してくれるのかな。

一応、入力しなくても自動で集計してくれるみたい。

--- 

作ったLINEbotで使用したスプレッドシートで使ったQUERY関数まとめ

`=QUERY(QUERY(A:C,"select A, sum(C) where A is not null group by A pivot B"),"select Col1,Col6,Col4,Col5,Col3,Col2")` 日付でソート

`=QUERY(A:C,"select A, sum(C) where C is not null group by A label A'日付', SUM(C)'計'")` 日毎の合計

`=QUERY(QUERY(A:C,"select YEAR(A),MONTH(A)+1, SUM(C) WHERE A IS NOT NULL GROUP BY YEAR(A),MONTH(A)+1 PIVOT B LABEL YEAR(A)'年',MONTH(A)+1'月'",0),"select Col1,Col2,Col7,Col5,Col6,Col4,Col3")` 月毎の合計　項目でソート

`=QUERY(A:C,"select YEAR(A),MONTH(A)+1, SUM(C) WHERE A IS NOT NULL GROUP BY YEAR(A),MONTH(A)+1 LABEL YEAR(A)'年',MONTH(A)+1'月',SUM(C)'計'",0)` 月毎の合計


