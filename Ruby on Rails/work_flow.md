## rails newしてからの大まかな流れ

作成するプロジェクトによってもさまざまだろうけど、railsで新規プロジェクトを立ち上げた時によく行う作業をまとめておこう。

### アプリ立ち上げ、Git準備編

- `bundle exec rails new <new_app> (--skip-action-mailer --skip-action-mailbox --skip-action-cable --skip-action-text)`
  - まずは新しいプロジェクト立ち上げ。これやる前に必要であればgithubで新規リモートリポジトリを作成しておく。)今から作り始めるプロジェクトで入らないであろう拡張機能はインストールをスキップしておいた方がいいかも。
- `cd <new_app>`
  - 言わずもがなだが、作成したディレクトリに入っておく
- `git init`
- `git remote add origin <remote_repo_name>`
- `git branch -M main`
- `git add .` → `git commit -m 'initial commit'` → `git push -u origin main`
- ブランチを切って作業用のブランチに切り替えて作業をしてもいいかも。その時は`git checkout -b <new_branch_name>`

---
### 作業環境準備編
（これはあくまで自分の場合だが）Gemfileの中のpumaの記述を見つけ、バージョンを4.3.6などに下げておく

続いて、必要であればerbからhamlの移行作業を行う。ERBを使う場合はそのままでよい。
- `'gem hamlit-rails', '~> 0.2.3'` >> Gemfile　>> `bundle install`
- `'gem html2haml', '~> 2.2.0'` >> Gemfile　>> `bundle install`
- `bin/rails hamlit:erb2haml` >> `y` 
- Gemfile >> ~`'gem html2haml', '~> 2.2.0'`~ >> `bundle install`
  - もうhtml2hamlは不要になったので消しておく

bootstrap導入

- `bin/yarn add bootstrap`
  - バージョンを指定したい時は指定して。bootstrap5からはjQueryとの依存関係がなくなったため、一緒にインストールしなくてもいいみたい。
- `bin/yarn add @popperjs/core`
  - popperjsは必要みたい。

- app/javascript/packs/application.jsに
  ```
  import "bootstrap"
  import "bootstrap/scss/bootstrap.scss"
  ```
  を追加

