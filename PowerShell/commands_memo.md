## コマンドメモ

PowerShellで使ったコマンドなどを、ひたすらメモとして残しておく。

### ランダムな英字を出力

PowerShellでrandomな英字を出力するために以下のスクリプトを教わったので、メモしておく。

` | clip `として出力先をクリップボードにすることもできる。

```
$(@('A'..'Z' + 'a'..'z') * 320  | Get-Random -Count $(320*52)) -join ""
```

### ipconfig

`ipconfig /flushdns`

でDNSのキャッシュをクリアできた

### nslookup xxxxxxxx

DNSサーバに名前解決を問い合わせる
