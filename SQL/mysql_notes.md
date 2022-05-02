## MySQL覚書

### MySQLにログイン

MySQLを使うにはterminal上でmysqlコマンドを使用する

```
mysql -u root -p
```
`-p`のあとは空白を開けずにEnter。そうするとパスワードを入力する画面になる。

ログインできると
```
mysql>

```
というようなプロンプトになる。

### ユーザーを作成したいとき

```
mysql> create user '<new_user_name>'@'<host_name>' identified by '<password>'
```

passwordは簡単なものだと怒られる

- 8文字以上
- 大文字も1文字以上
- 記号1文字以上
- 数字1文字以上

という条件みたいです。

---
### そもそもMySQL動かないときは

mysql入っているか確認

```
$ mysql --version
```

### Docker環境下でmysqlダンプファイルを読み込む

[こちら](https://jablogs.com/detail/31379)を参考にさせていただきました。

dumpファイルが巨大すぎると、タイムアウトでerror: 2がでる。

```
$mysql -u xxx -p < backup.sql
```
ではなく、mysqlに入って、

```
mysql> source backup.sql
```
とするとよい。


