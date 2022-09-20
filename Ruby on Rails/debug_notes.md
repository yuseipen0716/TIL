## デバッグについてのいろいろ

###  config.log_level

logを出す度合いを決めている設定。config/environment/development.rbの`config.log_level = :info`の記述をコメントアウトするか、info -> debugに変更すると、SQLなどのクエリのログ
もみられる。
