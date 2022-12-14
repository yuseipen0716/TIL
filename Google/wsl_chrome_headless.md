## WSL環境でchromeをheadlessで動かす準備

```
$ sudo apt update
$ sudo apt install libappindicator1 libappindicator3-1 fonts-liberation
$ curl -O https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
$ sudo dpkg -i google-chrome-stable_current_amd64.deb
```

chromeのversion確認
```
$ google-chrome --version
```
