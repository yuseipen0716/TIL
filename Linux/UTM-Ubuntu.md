## UTMを使用してUbuntuを立ち上げる

M1MacbookではVirtualBoxがまだ使えないみたい。

UTMというアプリケーションを使うと仮想環境下にUbuntuを立ち上げることができるみたい。

M1macはまだ仮想環境を立ち上げるような動作の安定性がいまいちだったり、ソフトが対応していないのもあって

仮想環境下でLinuxを立ち上げる方法などが載っている記事が少なかった。

今回UTMを使用してUbuntu serverを立ち上げてみたので、やり方や、ぶち当たったエラーなどを書いていく。

---

### UTMをhomebrewでインストール

` $ brew install --cask utm `

#### このcaskって何？

HomeBrew-CaskというHomeBrewの拡張機能らしい。

普段みたいに` brew install `でインストールするのと違って、

/Application/配下にインストールされる

#### そうするとどうなるの？

MacBookのLaunchpadから起動できる（GUIのアプリケーションとしてインストールできている。)

#### caskでインストールしたUTMをアップデート、アンインストール

` brew upgrade --cask utm `

` brew uninstall --cask utm `

***

### 手順まとめ

[こちら]: https://ubuntu.com/download/server/arm
[Ubuntu Server 20.04.2 LTS インストールの流れ]: https://linuc.org/docs/seminar/20210320_linuc1_02.pdf

#### UTM　仮想マシン設定作成編
1. まずはterminal上でhomebrewでUTMをインストール  
    1. ` $ brew install --cask utm `  
    1. ` $ brew upgrade --cask utm `
1. UTMを起動
1. Create a New Virtual Machine を選択
1. Name　のところを任意の仮想マシン名に変更
1. Style　のところは「Operating system」にして、アイコンを選択
1. <System>のところでArchitectureとメモリ容量を設定
    1. Architecture　は「ARM64(aaarch64)」に変更
    2. Systemの部分が「QEMU 6.0 ARM Virtual Machine」に変更される。ここはいじらなくてよし。
    3. Memoryの部分はサーバとして使うだけであれば1024とかでもいいけど、デスクトップ版も使いたい時は4096以上がよい
1. <Drives>のところで仮想マシンに接続するディスクを設定
    1. New Drives　をクリックして、OSをインストールするディスクを作成
    1. Interface は　VirtlO
    1. SIZE　は　デフォルトの10GBでも良いと思うが、20GBくらいあると安心
    1. Createをクリック
1. 同じく<Deives>から。インストール用のISOイメージをマウントするリムーバブルディスクを作成
    1. New Drive　をクリックして、リムーバブルディスクを作成していく。
    1. Removable　にチェックをいれる。そうするとInterface　がUSBとなるはず。
    1. Saveをクリックして作成したディスクを保存
1. それ以外の項目はデフォルトでOK（NAT+ポートフォワーディング等の設定をする場合はNetworkのところをいじるが、これについては別の時に。）
1. これで仮想マシンの初期設定はOK

#### Ubuntu server インストール編
    
[Ubuntu Server 20.04.2 LTS インストールの流れ] こちらのスライド２２枚目以降にインストールの流れが画像付きで紹介されています。
  
1. 準備。ARM64版Ubuntu serverインストール用のISOイメージをダウンロード
    1. ダウンロードは[こちら]から。今回利用するのはUbuntu 20.04.2 LTS
1. UTM画面にもどります。
1. 画面左側の仮想マシン一覧から先ほど作成した仮想マシンを選択。仮想マシンの詳細情報が出てくる。
1. CD/DVDのところが(empty)となっている。そこをクリックし、Browseを選択。
1. 先ほどダウンロードしたISOイメージを開いて、インストールメディアをマウント。
1. 再生ボタンを押して仮想マシンを起動すると、Ubuntu server　のインストール画面が立ち上がる。
1. Install Ubuntu Server　を選択 （インストール画面に移行するまで、ちょっと時間がかかります。）
1. 言語選択画面　　Englishを選択。他の言語がよければどうぞ。
1. キーボード選択画面　　USキーボードを使用しているのでEnglishを選択したけれど、日本語配列を使っている人はJapaneseにするのかな？
1. ネットワーク選択画面　　デフォルトはDHCP接続。固定IPがよければ　▶︎　を押す
1. プロキシ設定　　デフォルトでOK
1. ミラーアドレスの設定　デフォルトでOK
1. ストレージの設定　　デフォルトでOK　　Guided storage configration / Storage configrationどちらも
1. Profile 設定　　
    1. Your name → ユーザーの本名？
    1. Your server's name → サーバーの名前　（ex:ubuntu-server)
    1. Pick a username → ログインするユーザーネーム
    1. Choose a password → パスワード入力
1. SSHの設定　　Install OpenSSH server を[X]にする。　（カーソルで選択してEnter押す）　→ Done
1. Featured Server Snaps　　便利なオプションたち？ 特に選択せずDone
1. Install はじまる。　時間かかります。コーヒー淹れて飲んでも時間が余る。
1. 終わったら[Reboot] → Reboot選択すると途中から動かなくなることがあります。その時はUTMの電源ボタンみたいなところを押して強制終了。そのあと、仮想マシンを再起動する。その時は先ほどインストールメディアをマウントしたCD/DVDの部分で「Clear」を選択してインストールメディアをアンマウントしておく。
1. ここまで問題なく進められていると、Ubuntu Server が起動する。
    
#### Ubuntu デスクトップ環境を追加
    
1. ` sudo apt install tasksel `
1. ` sudo tasksel install ubuntu-desktop `
1. ` sudo reboot ` で再起動する。
1. デスクトップ環境を開いている状態でシャットダウンする時、デスクトップの電源アイコンを押してシャットダウンするとその後二度と再起動できない事件が発生した。これが原因じゃないかもしれないけど、今はterminalを開いて ` sudo shutdown -h now `　を入力してシャットダウンするようにしている。

    
#### やっていて困ったことエラーやバグ（？）
