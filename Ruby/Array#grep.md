## grepメソッド

配列にパターンを渡して、マッチしたものだけを要素に入れた配列に成形

```
pry(main)> AdControlledPage.first.url.split('/')
=> ["http:", "", "localhost:3000", "entertainment", "70269-2205"]
```

```
pry(main)> AdControlledPage.first.url.split('/').grep(/\d+-\d+/)
=> ["70269-2205"]
```
