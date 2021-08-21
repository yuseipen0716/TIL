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

1. まずはterminal上でhomebrewでUTMをインストール

