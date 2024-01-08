## make:testコマンドで作成されるテストファイルのテンプレ編集（非推奨）

個人用メモ

本来であれば、新しいtest.stubをプロジェクトのディレクトリに配置し、config/app.phpで読み込むstubsのパスをオーバーライドする方法がよさそう。

src/vendor/laravel/framework/src/Illuminate/Foundation/Console/stubs/test.stub
```php
<?php
declare(strict_types=1);

namespace {{ namespace }};

use Tests\TestCase;

class {{ class }} extends TestCase
{
    /**
     * A basic feature test example.
     *
     * @return void
     */
    public function test_example(): void
    {
        $response = $this->get('/');

        $response->assertStatus(200);
    }
}

```

念のため、元の形
```php
<?php

namespace {{ namespace }};

use Illuminate\Foundation\Testing\RefreshDatabase;
use Illuminate\Foundation\Testing\WithFaker;
use Tests\TestCase;

class {{ class }} extends TestCase
{
    /**
     * A basic feature test example.
     *
     * @return void
     */
    public function test_example()
    {
        $response = $this->get('/');

        $response->assertStatus(200);
    }
}

```
