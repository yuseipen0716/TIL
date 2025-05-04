# 家計botの自動デプロイを目指して

## 作業録
### 1. デプロイ用のSSHキーのsetup

```
ssh-keygen -t ed25519 -C "github-actions-deploy"
```

既に、id_ed25519という名前のキーペアがある場合

```
Enter file in which to save the key (/path/to/.ssh/id_ed25519):
```
のように効かれるので、別名で保存しておく。

```
Enter file in which to save the key (/path/to/.ssh/id_ed25519): /path/to/.ssh/github_actions_deploy
```

### 2. RaspberryPi側にdeploy用のユーザーを作成する
deployという名前のuserを作成しておく
```
sudo adduser deploy

# passwordなどを聞かれるため、設定する。フルネームなどはskip可
```

必要に応じてsudo権限の付与

```
# sudoグループに追加
sudo usermod -aG sudo deploy

# デプロイ時にDockerのコマンドを実行するため、
sudo usermod -aG docker deploy
```

1で作成したSSHのキーを設定

```
sudo mkdir -p /home/deploy/.ssh
sudo chown deploy:deploy /home/deploy/.ssh
sudo chmod 700 /home/deploy/.ssh

# 1で作成したキーペアのうち、公開鍵を追加する
sudo vim /home/deploy/.ssh/authorized_keys

# 権限設定
sudo chown deploy:deploy /home/deploy/.ssh/authorized_keys
sudo chmod 600 /home/deploy/.ssh/authorized_keys
```

デプロイ用ユーザーがアプリケーションディレクトリを操作できるようにする
```
sudo groupadd appdeployers
sudo usermod -aG appdeployers deploy
sudo usermod -aG appdeployers [現在ログイン中のuser_name]
sudo chown -R :appdeployers /path/to/kakeibo_on_rails # /home/<user>のようなディレクトリのgroupも変更する必要あり。ただ、その場合は、-Rで再帰的にしないように注意すること
sudo chmod -R g+rwx /path/to/kakeibo_on_rails
```

接続テスト。

deployというユーザーにSSHで接続可能かをチェック

```
# localマシンで
ssh -i ~/.ssh/github_actions_deploy deploy@raspberry_pi_ip
```

ログイン後、アプリケーションディレクトリに移動できるか、dockerコマンドを実行できるか等を確認する。

### 3. GitHub Secretsの設定
Settings >> Secrets and variables >> Actions から、以下のsecretsを追加

- `SSHPRIVATE_KEY`: 生成した秘密鍵の内容
- `SSH_HOST`: Raspberry PiのIPアドレスまたはホスト名
- `SSH_PORT`: SSHポート
- `SSH_USER`: Raspberry PiのSSHユーザー名


### 4. ワークフローのファイルを作成






