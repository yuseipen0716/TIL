## JSON鍵ファイルを環境変数化する

GoogleAnalyticsのReportingAPIを使用する際、認証にJSON形式の鍵ファイルを用いる形で実装した。

開発環境での動作は問題なかったが、「これ本番デプロイするときはどうやるんだろう」というのが疑問だった。

鍵ファイルをGitのバージョン管理に載せて、リモートリポジトリにpushするわけにはいかないだろうし、
本番サーバにsshするなりして、同様の鍵ファイルを作成するのも、なんだか事故が起こりそう。

できたら環境変数などに鍵の情報(client_emailとかprivate_key)とかを載せて、読み込む形にしたい。

調べてみると、あまり多くはないが記事がいくつか出てきた。なかなか理解が難しかったり、自分なりに色々試行錯誤した部分もあるので、あとで困らないようにメモを残しておく。<br><br>

### 手順（ざっくり）
1. JSON形式の鍵ファイルをbase64エンコーディングする
1. base64エンコーディングした鍵ファイルを環境変数(GOOGLE_SERVICE_KEY)として定義する
1. アプリケーション側でdecodeして認証を行う
<br><br>

#### JSON形式の鍵ファイルをbase64エンコーディング

JSONのファイルをbase64エンコーディングするにあたって、以下のようなrubyスクリプトを作成した。

```ruby
# base64_encode.rb
  1 require 'base64'
  2
  3 def encode_base64_from_file(file_path)
  4   # Open the file you want to encode
  5   data = File.open(file_path).read
  6
  7   # encode strict(改行コードを生成しない)
  8   puts Base64.strict_encode64(data)
  9   # for checking
 10   # puts Base64.strict_decode64(encoded)
 11 end
 12
 13 file_path = ARGV[0]
 14
 15 encode_base64_from_file(file_path)
 ```
 このスクリプトに鍵ファイルのファイルパスを渡して実行する
 ```
 $ ruby base64_encode.rb /path/to/Keyfile.json
 ```
 
 そうすると、エンコーディングされた文字列が出力されると思うので、copyするか、スクリプトを実行する際の出力をパイプでつないでclipボードに出力するようにしてあげる。<br><br>
 
 #### base64エンコーディングした鍵ファイルを環境変数(GOOGLE_SERVICE_KEY)として定義する
 
 base64エンコーディングされた鍵ファイルの文字列を`.env`ファイルに記載しておく。（本番サーバであれば`.bashrc`とかに。）
 
 今回はGoogleAnalyticsのReportingAPIを使用するので、GAのview_idも環境変数に入れておく。
 
 ```
 # .env
 
 # base64encodingされたJSON鍵ファイル
 GOOGLE_SERVICE_KEY = 'xxxxxXXXX0000....'
 # GAのview_id
 VIEW_ID = '00000000'
 ```
 <br><br>
 
 #### アプリケーション側で鍵をdecodeして認証を行う
 まず、完成版のコードを貼っておく
 
 ```ruby
 namespace :access_ranking do
  desc "JSONファイルにアクセスし、ページのパスとアクセス数を取得し、DBに格納"
  task :get_rank_data do
    #ランキングデータ取得処理
    # Load the client
    require "google/apis/analyticsreporting_v4"

    analytics = Google::Apis::AnalyticsreportingV4
    view_id = ENV['GA_VIEW_ID'] # GAのview_id
    google_service_key = Base64.strict_decode64(ENV['GOOGLE_SERVICE_KEY']) # base64encodingした鍵ファイルをdecodeしたもの
    scope = ['https://www.googleapis.com/auth/analytics.readonly']

    # Create a client object
    # 認証の操作。base64でencodingされた鍵ファイルの文字列をdecodeして認証に使用する。
    client = analytics::AnalyticsReportingService.new
    client.authorization = Google::Auth::ServiceAccountCredentials.make_creds(
      json_key_io: StringIO.new(google_service_key),
      scope: scope
    )
    
    # ここから下はデータを取得してくる処理。認証のことが知りたいだけなら読み飛ばしてOK
    
    # start_dateとend_dateは定数化しておく
    START_DATE = '30DaysAgo'
    END_DATE = 'today'
    date_range = analytics::DateRange.new(start_date: START_DATE, end_date: END_DATE)
    metric = analytics::Metric.new(expression: 'ga:uniquePageviews', alias: 'uniquePageviews')
    dimension = analytics::Dimension.new(name: 'ga:pagePath')
    order_by = analytics::OrderBy.new(field_name: 'ga:uniquePageviews', sort_order: 'DESCENDING')
    dimension_filters = analytics::DimensionFilterClause.new(
      operator: 'AND',
      filters: [
        analytics::DimensionFilter.new(
          dimension_name: 'ga:pagePath',
          operator: 'PARTIAL',
          not: 'true',
          expressions: ['ke']
        ),
      ]
    )

    report_request = analytics::GetReportsRequest.new(
      report_requests: [
        analytics::ReportRequest.new(
          view_id: view_id,
          metrics: [metric],
          date_ranges: [date_range],
          dimension_filter_clauses: [dimension_filters],
          dimensions: [dimension],
          order_bys: [order_by],
        )
      ]
    )
    response = client.batch_get_reports(report_request)
    data = response.reports.first.data
    puts "月間PV数: #{data.totals.first.values.first}"
    data.rows.each do |row|
      puts "#{row.metrics.first.values.first}: #{row.dimensions.first}"
    end
  end
end
 ```
 <br><br>
 
#### 認証の方法いろいろなパターン
##### ■ 鍵ファイルのパスをしていする方法
```ruby
analytics = Google::Apis::AnalyticsreportingV4
scope = ['https://www.googleapis.com/auth/analytics.readonly']
client = analytics::AnalyticsReportingService.new
client.authorization = Google::Auth::ServiceAccountCredentials.make_creds(
  json_key_io: File.open(ENV['GA_AUTH_PATH']),
  scope: scope
)
```
##### ■ 環境変数に必要な情報を記載して読み込む方法

[こちらを参照](https://github.com/googleapis/google-auth-library-ruby#example-service-account:~:text=Example%20(Environment%20Variables))

clientライブラリにやり方が載っていた。

##### ■ base64encodeした鍵ファイルをdecodeしてJSON.parseして使用する方法
各環境変数がどんなふうに読み込まれるのかは[こちら](https://github.com/googleapis/google-auth-library-ruby/blob/636e56042a7ac4963b09779f341654aa9b4c2f28/lib/googleauth/service_account.rb#L73)
に定義されている。

```ruby
decoded = Base64.strict_decode64(ENV['GOOGLE_SERVICE_KEY'])
parsed = JSON.parse(decoded)
client_id = parsed['client_id']
client_email = parsed['client_email']
account_type = parsed['type']
private_key = parsed['private_key']
project_id = parsed['project_id']
client = analytics::AnalyticsReportingService.new
auth = ::Google::Auth::ServiceAccountCredentials.make_creds(
  project_id: project_id,
  issuer: client_email,
  signing_key: OpenSSL::PKey::RSA.new(private_key.gsub(/\\n/, '\n')),
  scope: scope
)
client.authorization = auth
```
<br><br>

### 詰まった点

#### 鍵ファイルを読み込む方法と認証の方法が異なる

当たり前やん？って思うけど、以下の`json_key_io: File.open(ENV['GA_AUTH_PATH'])`のところをdecodeした文字列にしたらええやん、と思って

`json_key_io: File.open(ENV['GA_SERVICE_KEY'])`としてしまっていた。

文字列を読み込ませるには`json_key_io: StringIO.new(Base64.strict_decode64(ENV['GOOGLE_SERVICE_KEY']))` こう。

```ruby
client.authorization = Google::Auth::ServiceAccountCredentials.make_creds(
  json_key_io: File.open(ENV['GA_AUTH_PATH']),
  scope: scope
)
```

 
 
 
 
 


### GCPのAPIを呼び出すアプリケーションの認証メカニズムのベストプラクティス
Googleさんによると、GCPのAPIを呼び出すアプリケーションの認証メカニズムのベストプラクティスとして、サービスアカウントキーを使用しない新しい認証メカニズム、
[Workload Identity 連携](https://cloud.google.com/blog/ja/products/identity-security/enable-keyless-access-to-gcp-with-workload-identity-federation)なるものがあるらしい。

Workloar Identity連携については[こちらhttps://christina04.hatenablog.com/entry/workload-identity-federation)に図解でとても詳しく解説してくださっている方がいました。

### 参考
- [GCPのサービスアカウントのKeyfile.jsonを環境変数で管理する](https://zenn.dev/dashi296/articles/02a42890f0e6bf)
- [rubyからスプレッドシートに読み書きする](https://zenn.dev/cumet04/scraps/c471e4aacaa448)
- [google-auth-libraly-ruby UserCredential](https://github.com/googleapis/google-auth-library-ruby#example-service-account:~:text=Google%20Compute%20Engine.-,User%20Credentials,-The%20library%20also)



