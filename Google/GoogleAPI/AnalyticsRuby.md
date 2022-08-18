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
