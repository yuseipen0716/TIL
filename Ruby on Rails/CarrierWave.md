## gem carrierwaveまとめ

railsのプロジェクトで画像のアップロードにcarrierwaveを使っていたので、使い方を調べた。

今回は基本的な使い方ではないかもしれない、console上での使用 + 画像ファイルは外部のURLから取得するような形での実装であったため少々調べるのに時間がかかった。

``` ruby
# article.rb

mount_uploader :image, ThumbnailUploader
```

``` ruby
# console

pry(main)> article = Article.new
=> #<Article:0x00005637053ee760 id: nil, thumbnail: nil>

article.remote_image_url = "https://<image_url>"
=> "https://<image_url>"

article.save
=> true
```

uploaderをmountするときの:image部分が:fileとかなら、remote_file_urlのようにメソッド名が変わるので注意。
