# docker_ruby3_mysql8
Docker環境構築 ruby3(rails7を想定)、mysql8

[参考サイト](https://qiita.com/neet244/items/8c5ed68fdc28121eecd7)

## Rails アプリケーション新規作成
```
docker-compose run web rails new . --force --no-deps --database=mysql --skip-test --webpacker
docker-compose build
```

## データベース接続
```
default: &default
  adapter: mysql2
  encoding: utf8mb4
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
  username: <%= ENV.fetch("MYSQL_USERNAME", "root") %>
  password: <%= ENV.fetch("MYSQL_PASSWORD", "password") %>
  host: <%= ENV.fetch("MYSQL_HOST", "db") %>

development:
  <<: *default
  database: rails_practice_development
```

```
docker-compose run web rake db:create
docker-compose up -d
docker-compose exec web bash

※必要ならば
rails -s
```
## テーブルを作ってみる
```
rails g scaffold customers name:string email:string password:password_digest status:integer

rails db:migrate
```


## 作成したdockerコンテナをすべて削除する
```
docker-compose down --rmi all --volumes --remove-orphans
```

