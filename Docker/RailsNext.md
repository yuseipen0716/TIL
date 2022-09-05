## Docker下でRails、Next.jsの環境をつくる

```
ファイル構成( 初期 )
|--docker-compose.yml
|--api/----Dockerfile
|       |--Gemfile
|        --Gemfile.lock(何も入力せず）
|
|--front/Dockerfile      
```

```
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

