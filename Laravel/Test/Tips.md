## PHPUnit テストに関するTips

### テストをスキップする
何らかの事情で、現状の実装だとテストが通らない場合などにテストケースをスキップしたくなる時がある。

```php
/**
 * skip したいテストケース
 *
 * @return void
 */
public function testSkipShitai(): void
{
  $this->markTestSkipped('のっぴきならない理由でskip');
  ...(ここより下は何書いていてもOK...?)
}
```

実行すると

```shell
php artisan test

- skip shitai -> のっぴきならない理由でskip
```
