### SQLの命令言語の分類

SQLの命令言語は大きく四つに分類できる。また、四つに分類する前に以下のような二つに分類できるので、まとめて記載する。

1. データベースにデータの出し入れの指示を出す立場の人のための命令言語
      1. Data Manipulation Language(DML):データ操作言語 ex.SELECTやUPDATE等の4大命令
      1. Transaction Control Language(TCL):トランザクション制御言語 ex.COMMITとかROLLBACK等
1. ↑の人がデータの操作をしやすいように、データベースを作成したり、その他設定を行う立場のための命令言語
      1. Data Definition Language(DDL):データ定義言語 CREATE,ALTERなど
      1. Data Control Language(DCL):データ制御言語 ex.GRANT,REVOKE

