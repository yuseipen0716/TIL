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

docker環境下でmysqlに入ろうとしたとき、うまく入れないことがあった。

その時打ったのは`mysql -u root -p`

```
$ docker-compose ps
```
で動いているプロセスを確認。mysqlのコンテナID（ID）をコピー

```
$ mysql -h <mysqlのコンテナID> -u root -p
```

これで入れた。

### Docker環境下でmysqlダンプファイルを読み込む

[こちら](https://jablogs.com/detail/31379)を参考にさせていただきました。

(ページが無くなってしまうと困るためメモしておく）

(docker-compose upしておいてください)

コンテナを立ち上げているディレクトリで

```
docker ps
```
して、MySQLDockerのコンテナIDをcopyしておく

```
mysql -h <今コピーしたmysqlコンテナのID> -u root -p
```

これで、 mysql> みたいなプロンプトになったらOK

```
mysql> show databases;  (mysql> の部分は打たない)
```

```
mysql> use <今から使うdatabase名>;  (mysql> の部分は打たない)
```
バックアップ復元のもとの、dumpファイルは一旦railsアプリケーションのディレクトリに入れておく(appとか、pulicとかが並んでいるところ)

```
mysql> source <dumpファイルのファイル名>;
```

（とっっっても時間がかかるので、コーヒーでも飲んで待つ。)


---

### dumpファイルからimport後、文字化けしている

mysqlに入って`show variables like "chara%"`と打つと、現在の環境で使われている文字コードの一覧がでてくる。

### mysqldumpやりかた

下記を参考にやってみた。

- > [mysqldumpでバックアップ・リストアする](https://www.tohoho-web.com/ex/mysql-mysqldump.html)
- > [MySQLバックアップのとり方](https://qiita.com/iika0220/items/01d4b8bde4c06cf13fec)


## コマンド 参考リンク

[よく使うMySQLコマンド&構文集](https://qiita.com/CyberMergina/items/f889519e6be19c46f5f4#%E3%83%86%E3%83%BC%E3%83%96%E3%83%AB%E4%B8%80%E8%A6%A7%E3%81%AE%E8%A1%A8%E7%A4%BA)



