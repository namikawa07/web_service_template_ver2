# README

## 参考資料
URL: https://qiita.com/maru401/items/2f5815c40fcf6d2d80d6

## development
URL: http://localhost:3200/

## pgAdmin
URL: http://localhost:8080/

id: postgres@linuxhint.com

password: 1235678

## docker立ち上げ

```
$ docker-compose build
$ docker-compose up
```

## docker終了

```
$ docker-compose down
```

## webpacker
変更を検知して自動的にビルドやブラウザのリロードを行うには、webpack-dev-serverを使う
#### 使用方法
rails sを走らせているときに別プロセスで
```
$ ./bin/webpack-dev-server
```
を実行

## 開発環境

### 1.新しいgitリポジトリを作成する
web_service_templateの`Use this template`を押す
￼
リポジトリ名にサービス名を記入しcreateする

作成したらcodeからHTTPSを選びコードをもってきたい場所にcloneする


￼
```
git clone HTTPSにコード
```

### 2.herokuにあげるstaging環境とproduction環境を作成する

作業するルートディレクトリで

```
$ heroku login
```
でherokuにログインする

```
$ heroku container:login
```
Heroku containerにログインする

・staging環境を作る

```
$ heroku create サービス名 --remote staging
```

・production環境を作る

```
$ heroku create サービス名 --remote prod
```
サービス名はURLとかでも表示されるのでgithubで使ってる名前が良い
※アンダーバー使えないので注意！

もし名前を間違った時
```
$ heroku apps:destroy --app アプリ名
```
これでheroku上のアプリは削除されるので初めから作る

作成できたら
```
$ git config --list
```
で確認
staging環境とprod環境のherokuのgitができている

・webpackをコンパイルする

```
$ docker rm $(docker ps -aq) --force
$ docker-compose run --rm web rails assets:precompile RAILS_ENV=production
```
前に使っていたTemplateのdockerが残っている可能性があるので一度全てのdockerをクリーンにしてからdockerを新しく立ててwebpackerをコンパイルしている

・DBを設定する

```
$ heroku addons:create heroku-postgresql:hobby-dev --app アプリ名
```
これもstagingとprodの2つやる


### 3.staging環境とprod環境をherokuであげてみる

```
$ heroku login
& heroku container:login
```

Dockerを落とす
```
$ docker-compose down
```

dockerコンテナをherokuにpushする
```
$ heroku container:push web --app アプリ名
```

データベースのmigrate (createは必要なし)
```
$ heroku run rails db:migrate --app アプリ名
```
webpackを反映させる
```
$ heroku run rails assets:precompile --app アプリ名
```
コンテナを起動する
```
$ heroku container:release web --app アプリ名
```
アプリを開く
```
$ heroku open --app アプリ名
```
#### server
ruby 2.6.5p114
Rails 6.1.4.1

#### front
vue.js (実装中)
nuxt.js (実装中)

#### database
postgresql

#### heroku
postgreSQL：hobby-dev

uokun-web-app：herokuのアプリにつけた名前

https://uokun-web-app.herokuapp.com/


#### other
rubocop 1.23.0
rspec-rails 4.0.2
yarn 1.22.15


This README would normally document whatever steps are necessary to get the
application up and running.

Things you may want to cover:

* Ruby version

* System dependencies

* Configuration

* Database creation

* Database initialization

* How to run the test suite

* Services (job queues, cache servers, search engines, etc.)

* Deployment instructions

* ...
