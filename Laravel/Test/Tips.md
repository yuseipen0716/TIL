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

### $responseから値を取得する
PHPUnitのテストを書いていて、$responseで得られる値から、その中のpropertyにアクセスして値を取得したい場合、以下のように行うことができた。

```php
$response = $this->postJson('some/path');

$responseArray = $response->json();
$body = $responseArray['body']
print_r($body);
```
