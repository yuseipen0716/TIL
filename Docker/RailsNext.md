## Docker下でRails、Next.jsの環境をつくる

ファイル構成( 初期 )
```
|--docker-compose.yml
|--api/----Dockerfile
|       |--Gemfile
|        --Gemfile.lock(何も入力せず）
|
|--front/Dockerfile      
```

docker-compose.yml
```yaml
version: '3.7'

services:
  db:
    image: mysql:5.7
    command: --character-set-server=utf8mb4 --collation-server=utf8mb4_unicode_ci
    environment:
      MYSQL_ROOT_PASSWORD: password
      MYSQL_DATABASE: sample
      MYSQL_PASSWORD: password
    ports:
      - 4306:3306
    volumes:
      - mysql-db:/var/lib/mysql
  api:
    tty: true
    depends_on:
      - db
    build:
      context: ./api/
      dockerfile: Dockerfile
    ports:
      - 3000:3000
    volumes:
      - ./api:/app
    command: /bin/sh -c "rm -f tmp/pids/server.pid && bundle exec rails s -p 3000 -b '0.0.0.0'"
  front:
    build:
      context: ./front/
      dockerfile: Dockerfile
    volumes:
      - ./front/app:/usr/src/app
    command: 'yarn dev'
    ports:
      - "8000:3000"
volumes:
  mysql-db:
    driver: local
```

/front/Dockerfile
```Dockerfile
FROM node:14-alpine
WORKDIR /usr/src/app
```

/api/Dockerfile
```Dockerfile
FROM ruby:2.7

ENV LANG=C.UTF8 \
  TZ=Asia/Tokyo

WORKDIR /app
RUN apt-get update -qq && apt-get install -y nodejs default-mysql-client
COPY Gemfile /app/Gemfile
COPY Gemfile.lock /app/Gemfile.lock
RUN bundle install

# Start the main process.
CMD ["rails", "server", "-b", "0.0.0.0"]
```
Gemfile
```ruby
source 'https://rubygems.org'
gem 'rails', '~> 6.0.3', '>= 6.0.3.2'
```

Gemfile.lockは作成だけしておく。

上記のファイルが揃ったらビルド
```
$ docker-compose build
```

ディレクトリはそのままで
```
$ docker-compose run --rm front yarn create next-app .
```

Nextアプリケーションを立ち上げる
```
$ docker-compose up
```
localhost:8000にアクセス

Railsの方の準備
```
$ docker-compose run --rm api bundle exec rails new . --api -d mysql
```

```
default: &default
  adapter: mysql2
  encoding: utf8mb4
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
  username: root
  password: password
  host: db

development:
  <<: *default
  database: sample

# Warning: The database defined as "test" will be erased and
# re-generated from your development database when you run "rake".
# Do not set this db to the same as development or production.
test:
  <<: *default
  database: app_test
```

Railsのサーバを立ち上げる
```
$ docker-compose up --build
```



#### 参考
Next.js + Ruby on Rails(APIモード) on Docker 構築手順(https://zenn.dev/taku1115/articles/6c9fa97ab37e38)
