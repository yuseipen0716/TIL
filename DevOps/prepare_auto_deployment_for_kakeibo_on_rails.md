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
のようにきかれるので、別名で保存しておく。

```
Enter file in which to save the key (/path/to/.ssh/id_ed25519): /path/to/.ssh/github_actions_deploy
```

※ ワークフローの中で動かす場合、パスフレーズを求められるとPermission deniedなどのエラーになるため、パスフレーズは設定せず、キーペアを作成する。

### 2. RaspberryPi側にdeploy用のユーザーを作成する
deployという名前のuserを作成しておく
```
sudo adduser deploy

# passwordなどを聞かれるため、設定する。フルネームなどはskip可
```

必要に応じてsudo権限の付与（今回は必要なかったので省略）

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

### 4. repositoryのDeployKeyを設定する
自動デプロイのワークフローの中で、`git pull`などを実行するため、必要。

ワークフローの中で実行するため、パスフレーズは設定せず作成しておく

#### キーペアの作成
RaspberryPiに、deployユーザーでログインした状態で実行。

```
ssh-keygen -t ed25519 -f ~/.ssh/github_deploy_key -C "github-deploy-key"
```

#### GitHub上でDeployKeyの登録
↑ こちらで作成したキーペアのうち、公開鍵の情報をrepositoryのDeployKeyに登録しておく。

※ 今回は、`git fetch`や`git pull`などの読み取り操作のみ行うため、Read-onlyの権限にした

#### ~/.ssh/config編集
今回作成した鍵を使用するようにする

```config
Host github.com
  HostName github.com
  User git
  IdentityFile ~/.ssh/github_deploy_key
  IdentitiesOnly yes
```

#### 動作確認
```
ssh -T git@github.com
```
を実行したとき、パスフレーズなしで、テストに成功することを確認する。（Hi! xxx のようなメッセージが返ってきたらOK）

### 5. ワークフローのファイルを作成
```yaml
name: Deploy Production

on:
  push:
    branches: [ main ]

jobs:
  deploy_production:
    runs-on: ubuntu-latest
    steps:
      - name: Install SSH key
        uses: shimataro/ssh-key-action@v2
        with:
          key: ${{ secrets.SSH_PRIVATE_KEY }}
          name: id_ed25519
          known_hosts: unnecessary
          if_key_exists: replace

      - name: Adding Known Hosts
        run: ssh-keyscan -p ${{ secrets.SSH_PORT }} ${{ secrets.SSH_HOST }} >> ~/.ssh/known_hosts

      - name: Deploy Production with build check
        run: |
          ssh -p ${{ secrets.SSH_PORT }} ${{ secrets.SSH_USER }}@${{ secrets.SSH_HOST }} '
            cd kakeibo_on_rails &&
            git checkout . &&
            git fetch origin main &&

            # サービスを停止しておく
            docker-compose -f docker-compose.production.yml down &&

            # Gemfile関連に変更があるか確認
            if git diff --name-only HEAD..origin/main | grep "Gemfile\|Gemfile.lock"; then
              echo "Gemfile changes detected, building containers..."
              git pull origin main &&
              docker-compose -f docker-compose.production.yml build &&
              docker-compose -f docker-compose.production.yml up -d
            else
              echo "No Gemfile changes, regular deploy"
              git pull origin main &&
              docker-compose -f docker-compose.production.yml up -d
            fi
          '
```

### 6.動作確認
成功。（何度も失敗した末、、）
![image](https://github.com/user-attachments/assets/a39a168b-547e-43b9-94f7-497929059113)



## 詰まったところ
### ssh接続できない（Permission denied）
作成したキーペアが、パスフレーズを求めるものだった。

### git pull実行時にエラーが発生
原因としては同上。パスフレーズを求められてしまったため、スクリプトの続きが実行できない状態になっていた。



