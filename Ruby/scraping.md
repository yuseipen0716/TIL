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


--- 
### 参考
- [Seleniumクイックリファレンス](https://www.seleniumqref.com/api/webdriver_gyaku.html)
- [Seleniumチートシート [Ruby]](https://morizyun.github.io/web/selenium-cheat-sheet.html) 
