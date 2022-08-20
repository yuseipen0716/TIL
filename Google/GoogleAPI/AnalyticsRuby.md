## GoogleAnalyticsReportingAPIをRubyで叩く

GoogleAnalytics（以下GA）で集計しているサイトのPV数などを取得するべくAPIを叩きたかったが、自分の勉強不足でかなり苦戦したので、こちらにメモを残しておく。

なにはともあれ、大まかにできたコードを載せておく。railsのrake taskで実装したときのコードです。

```ruby
namespace :access_ranking do
  desc "JSONファイルにアクセスし、ページのパスとアクセス数を取得し、DBに格納"
  task :get_rank_data do
    #ランキングデータ取得処理
    # Load the client
    require "google/apis/analyticsreporting_v4"

    # GAの管理画面>>ビュー>>ビューの設定をクリックすると、ビューIDが確認できる。環境変数等で管理しておく。
    view_id = 'VIEW_ID'
    scope = ['https://www.googleapis.com/auth/analytics.readonly']
    analytics = Google::Apis::AnalyticsreportingV4

    # Create a client object
    # auth.jsonはサービスアカウント作成時に生成した鍵のJSONファイル
    client = analytics::AnalyticsReportingService.new
    client.authorization = Google::Auth::ServiceAccountCredentials.make_creds(
                             json_key_io: File.open('lib/tasks/auth.json'),
                             scope: scope
    )

    date_range = analytics::DateRange.new(start_date: '30DaysAgo', end_date: 'today')
    metric = analytics::Metric.new(expression: 'ga:pageviews')
    dimension = analytics::Dimension.new(name: 'ga:pagePath')
    request = analytics::GetReportsRequest.new(
      report_requests: [analytics::ReportRequest.new(
        view_id: view_id, metrics: [metric], dimensions: [dimension], date_ranges: [date_range]
      )]
    )
    response = client.batch_get_reports(request)

    pp response.reports
  end
end
```

鍵ファイルのpathや、start_date、end_dateなども定数化しておいた方が、今後の変更に強くなる。

あとは、orderByで並び替えをして、ランキング形式にしてデータを取得できるようにするなど、工夫はいっぱいできる。

GCPでの操作（プロジェクト作成とか、認証情報の入力とか）のことも備忘録として残しておいた方がよさそう。

### そういえば

google-api-clientのgemをinstallしようと思い、Gemfileに記述後、bundle installしたら、下記のようなメッセージが出た。

```
Post-install message from google-api-client:      
*******************************************************************************    
The google-api-client gem is deprecated and will likely not be updated further.   

Instead, please install the gem corresponding to the specific service to use.   
For example, to use the Google Drive V3 client, install google-apis-drive_v3.       
For more information, see the FAQ in the OVERVIEW.md file or the YARD docs.  
*******************************************************************************  
```

詳細については[こちら](https://github.com/googleapis/google-api-ruby-client/blob/main/google-api-client/OVERVIEW.md) に記載されていた。

google-api-clientとして使うというより、各API個別でrequireして使うような感じになるみたい。上記のコードもそのようにして記載した。

### GCPでの準備
時系列はめちゃくちゃだが、ここにメモを残しておく

- プロジェクトを作成


### 参考
- [google-api-ruby-client/generated/google-apis-analyticsreporting_v4/OVERVIEW.md](https://github.com/googleapis/google-api-ruby-client/blob/main/generated/google-apis-analyticsreporting_v4/OVERVIEW.md)
- [GoogleAnalytics>>ReportingAPI>>v4>>レポートの作成](https://developers.google.com/analytics/devguides/reporting/core/v4/basics?hl=ja#reports)
- [RubyでGoogle アナリティクス Reporting API v4を使って、本日のページごとのユーザ数を取得する](https://shingo-sasaki-0529.hatenablog.com/entry/google_analytics_api_by_ruby)
- [Google Analytics APIを使ってブログのPV数を見る](https://www.nogawanogawa.work/entry/google_analytics_api)




