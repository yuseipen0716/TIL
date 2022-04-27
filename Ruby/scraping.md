## スクレイピング　Rubyで実装してみる

スクレイピングについて学びながら、Seleniumを使用しながら実装してみたい。

[selenium-webdriverのgem](https://github.com/SeleniumHQ/selenium/tree/trunk/rb)があったので、こちらを使います。

インストール方法は通常のgemと同じ。

今回は作業するディレクトリで`bundle init`してGemfileを生成してGemfile内に`gem 'selenium-webdriver'`を記載後`bundle install`

### 準備

まずは今使っているChromeのバージョンを確認。左上のChromeのボタンを押して「Google Chromeについて」をクリックすればバージョンがわかる。

それに対応したchromedriverをインストールする必要がある。

こちらの[chromedriver_storage](http://chromedriver.storage.googleapis.com/index.html)から、自分のchromeバージョンに対応したdriverをダウンロードし、解凍する。

それから

```
$ sudo mv <解凍したchromedriverのパス> /usr/local/bin
```
↓パスを通す

```
$ export PATH="/usr/local/bin:$PATH"
```

ちゃんとパスが通って、ファイルが存在するか確認

```
$ which chromedriver
->/usr/local/bin/chromedriver
->OK!
```

### サンプルコード

以下、簡単なサンプルプログラムを置いておく。（Yahoo!で「CPU」と検索して結果を表示後、current_urlを吐き出してdriverを閉じるだけ）

正直これでも割と詰まって時間がかかってしまったので、詰まった点もサンプルコードの下に書き残しておく。

``` ruby
require 'selenium-webdriver'

@wait_time = 10
@timeout = 4

# Seleniumの初期化
Selenium::WebDriver.logger.output = File.join("./", "selenium.log")
Selenium::WebDriver.logger.level = :warn
driver = Selenium::WebDriver.for :chrome
# driverがきちんと起動したか、暗黙的な待機を入れる
driver.manage.timeouts.implicit_wait = @timeout
wait = Selenium::WebDriver::Wait.new(timeout: @wait_time)

# yahooを開く
driver.get('https://www.yahoo.co.jp/')

# ちゃんと開けているか確認するため、page_loadのwaitを入れる
driver.manage.timeouts.page_load = @wait_time

# ブラウザでさせたい動作を記載する
# 今回は検索欄に'CPU'と入力して、検索ボタンを押す処理

begin
  # 検索欄を取得し、値を入力
  driver.find_element(:css, 'input[type="search"]').send_keys 'CPU'
  # 検索ボタンを取得し、クリック
  driver.find_element(:css, 'button[type="submit"]').click
rescue Selenium::WebDriver::Error::NoSuchElementError
  p 'no such element error!!'
  return
end

# ここで待機を入れないと、ページ遷移を待たずしてcurrent_url(https://www.yahoo.co.jp/')を吐き出してdriverが落ちるので、
# page_titleを明示的に指定してwaitをかけている。
# この部分、もっとスマートに書きたい。今後の課題。
page_title = '「CPU」の検索結果 - Yahoo!検索'
wait.until {driver.title == page_title}

# current_pageのURLを取得して出力する処理
cur_url = driver.current_url
puts cur_url

# 忘れず、driverを閉じること!
driver.quit
```

### 詰まった点
- 当然ではあるが、スクレイピング対策なのかクラス名はランダム生成のような値であったため、クラス名で要素を取得するのはやめた。どうやって取得しようか悩んだ。
  - ↑これについてはcssセレクタで取得することで落ち着いた。
- 待機について。初めにサンプルプログラムを動かした際には検索ボタンをクリックした後のページ遷移を待つための待機処理を入れていなかったため、current_urlを吐き出させたところ、yahooのトップページのまま吐き出されていた。wait.untilには色々な指定の仕方があったが、クリック前後に処理を入れる、とかページ遷移前後に処理を入れるような操作をする場合、ライブラリ内のクラスを継承する形で実装しないといけない(?)ようだったので、別の方法(今回とった方法)で実装した。もっとスマートに書きたいところ。


### shadow_root

取得したい要素があって、classやcssセレクタを正しく(?)指定しても全然取得できない要素があった。Xpathを指定してもとれなくて、なんじゃこりゃってなった。

dev_toolsを見てみる。サイトを更新して、入場した時の状態で確認すると、見たい情報は`shadow_root`というところに格納されていて、サイトに入った時は見えないようになっていた。

どうやらこのshadow_rootの中身を取得するには2ステップくらい踏む必要があるみたいだった。
``` ruby
# まずは普通にサイトに入る
driver.get('http://watir.com/examples/shadow_dom.html')
```

``` ruby
# shadow_rootの親要素を取得する
shadow_host = driver.find_element(id: 'shadow_host')
```

``` ruby
# 先ほど取得したshadow_rootの親要素にshadow_rootメソッドを当てる
shadow_root = shadow_host.shadow_root
```

``` ruby
# あとは普通に要素を取得することができる
shadow_content = shadow_root.find_element(id: 'shadow_content')
```

### Xpath逆引き

dev_toolsのconsoleに$x('<xpath>')と入力する。
  
### ページ遷移のための処理メモ
  

``` ruby
# ========== ページ遷移のための処理 ==========
# 次へ、のリンクしかない(初めのページ)はほかのページとxpathが違うので、下記の通り、2パターンのxpathを
# 拾えるようにする。no such elementsで例外が出ないように、rescueでnilにしてあげる。
begin
  button_single = driver.find_element(:xpath, "")
rescue
  nil
end

begin
  button_double = driver.find_element(:xpath, "")
rescue
  nil
end

# 取得できたボタンのうち、どちらかが「次へ」ボタンであればroop
while button_single.text == "次へ" || button_double.text == "次へ"
  # button_singleをifの条件に書くと少し厄介なことになるので、doubleのほうでひっかける。
  # (2ページ目以降、つまり前へ、次へどちらのボタンもあるページではbutton_singleのxpathが
  # 前へ、次へのbuttonタグを内包する配列のxpathとなっているため、button_singleはnilにならず、前へボタンを取得してしまい、
  # ページが進まなくなる)
  if button_double
    button_double.click
  else
    button_single.click
  end
  sleep 1

  # デバッグ用にURLを吐き出すようにしてある。実際に運用する際は消すこと。
  puts driver.current_url

  # 最終ページ(前へボタンしかない状態)でもbutton_doubleの変数の値が残ってしまい、roopから抜けないので、ここでreset
  button_single = nil
  button_double = nil
  begin
    button_single = driver.find_element(:xpath, "")
  rescue
    nil
  end
  
  # デバッグ用にボタンテキストを吐き出すようにしてある。実際に運用する際は消すこと。
  puts button_single.text

  begin
    button_double = driver.find_element(:xpath, "")
  rescue
    nil
  end

  break if button_double == nil

  # デバッグ用にボタンテキストを吐き出すようにしてある。実際に運用する際は消すこと。
  puts button_double.text
end
```


--- 
### 参考
- [Seleniumクイックリファレンス](https://www.seleniumqref.com/api/webdriver_gyaku.html)
- [Seleniumチートシート [Ruby]](https://morizyun.github.io/web/selenium-cheat-sheet.html) 
- [Selenium webdriverよく使う操作メソッドまとめ](https://qiita.com/mochio/items/dc9935ee607895420186)
