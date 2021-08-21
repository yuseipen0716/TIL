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
  
1. 準備。ARM64版Ubuntu serverインストール用のISOイメージをダウンロード
    1. ダウンロードは[こちら]から。今回利用するのはUbuntu 20.04.2 LTS
1. UTM画面にもどります。
1. 画面左側の仮想マシン一覧から先ほど作成した仮想マシンを選択。仮想マシンの詳細情報が出てくる。
1. CD/DVDのところが(empty)となっている。そこをクリックし、Browseを選択。
1. 先ほどダウンロードしたISOイメージを開いて、インストールメディアをマウント。
1. 再生ボタンを押して仮想マシンを起動すると、Ubuntu server　のインストール画面が立ち上がる。
1. Install Ubuntu Server　を選択 （インストール画面に移行するまで、ちょっと時間がかかります。）
1. 
