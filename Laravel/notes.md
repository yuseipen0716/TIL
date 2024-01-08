## memo

### print_rなどでbooleanを出力してもtrue/falseにならない
Laravelが、なのかPHPが、なのかちょっと調べ切れていないが、`print_r(hoge === null);`のようにbooleanの値を標準出力に出そうとすると、trueなら`1`が出力され、falseの場合はなにも出力されない。

そのため、非常にデバッグがしにくいので、以下のようにするようにした。

```php
print_r(hoge === null ? 'true' : 'false');
```
