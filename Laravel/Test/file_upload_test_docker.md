## Docker環境におけるファイルアップロードのテストのつまりどころ

[Laravel 9.x HTTPテスト>>ファイルアップロードのテスト](https://readouble.com/laravel/9.x/ja/http-tests.html#:~:text=%E8%AA%8D%E8%AD%98%E3%81%97%E3%81%BE%E3%81%99%E3%80%82-,%E3%83%95%E3%82%A1%E3%82%A4%E3%83%AB%E3%82%A2%E3%83%83%E3%83%97%E3%83%AD%E3%83%BC%E3%83%89%E3%81%AE%E3%83%86%E3%82%B9%E3%83%88,-Illuminate%5CHttp%5CUploadedFile) を参考に

```php
    public function test_avatars_can_be_uploaded()
    {
        Storage::fake('avatars');

        $file = UploadedFile::fake()->image('avatar.jpg');

        $response = $this->post('/avatar', [
            'avatar' => $file,
        ]);

        Storage::disk('avatars')->assertExists($file->hashName());
    }
```
こんな感じのテストを書いたとき、

```
   PHPUnit\Framework\ExceptionWrapper 

  Call to undefined function Illuminate\Http\Testing\imagecreatetruecolor()

  at vendor/laravel/framework/src/Illuminate/Http/Testing/FileFactory.php:77
     73▕             $extension = in_array($extension, ['jpeg', 'png', 'gif', 'webp', 'wbmp', 'bmp'])
     74▕                 ? strtolower($extension)
     75▕                 : 'jpeg';
     76▕ 
  ➜  77▕             $image = imagecreatetruecolor($width, $height);
     78▕ 
     79▕             call_user_func("image{$extension}", $image);
     80▕ 
     81▕             fwrite($temp, ob_get_clean());


  Tests:  1 failed, 1 passed
  Time:   4.53s
```
のようなエラーが出た。

GDという拡張モジュールが足りない？ようだった。

Dockerfileのapt-get installしている部分に以下を追加。

```
RUN apt-get update &&\
  # JPEG 対応
  apt-get install -y libpng-dev libjpeg62-turbo-dev &&\
  docker-php-ext-configure gd --with-jpeg &&\
  docker-php-ext-install -j$(nproc) gd
```

上記を追加後、`docker compose up -d --build`


