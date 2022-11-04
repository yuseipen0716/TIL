## ログ監視いろいろ

ラズパイでSSHサーバを立てているので、悪意のある攻撃者からのアクセスの形跡などがないか、調べたい。

`/var/log`内にいろいろなログファイルがあるが、それらにどのようなログが吐きだされているかも改めて勉強。

### auth.log
先に記述するものは多いと思うが、自分がいちばん見てみたかったのは、auth.log

ここには、システムへのログイン履歴などが格納されるみたい。

#### ログインに成功したユーザーのIPアドレスを出力

ログインが成功すると、

```
Accepted publickey for <username> from <from_ip_address> port <port_num> ssh2: RSA xxxx
```

のような表示になるようなので、まずは自分以外の人がログインしていないかを確認する。

```
grep -E '[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}' /var/log/auth_cp.log | grep -E 'Accepted' | cut -d ' ' -f 11 | sort | uniq
```

こちらを実行すると、
```
xxx.xx.xxx.xxx
ooo.ooo.ooo.ooo
from
```
というように、ログインに成功したIPアドレスが表示される。

今回確かめた際は自分以外のユーザーのログインは認められなかった。（これらの監視はcronjobにして、おかしなアクセスがあったときはalertを出すような方法を考えたい。。）

#### ログインに失敗したユーザーのIPアドレスを出力

```
grep -E '[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}' /var/log/auth_cp.log | cut -d ' ' -f 8 | grep -E '[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}' | sort | uniq
```

数を数えるならこっち
```
grep -E '[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}' /var/log/auth_cp.log | cut -d ' ' -f 8 | grep -E '[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}' | sort | uniq | wc -l
```


### 参考
- [[メモ]Linuxの主なログファイル](https://qiita.com/Yorinton/items/897c1ccd6797a7df7805)
- [自宅の公開sshサーバの不正アクセスログを調査してみた](https://qiita.com/ydetectiveu007/items/f94745ed0a114b93da1b)


