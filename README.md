# 環境構築

* リポジトリをクローンする
* ディレクトリ名を変更し、ディレクトリに移動する
* プロジェクト名を設定する
* サーバ構築、 bundle install
* Rails new
* bundle install
* データベースの接続設定
* データベースを作成する
* 起動確認
* github でに push する

## リポジトリをクローンする

```
$ git clone git@github.com:gupipo/start_project.git
```

## ディレクトリ名を変更し、ディレクトリに移動する
ディレクトリ名は自由

```
$ mv start_project example
$ cd example
```

## プロジェクト名（フォルダ名）を設定する
`Dockerfile` の 18 行目にフォルダ名を設定する

```
ENV app_name start_project
```

↑ を変更する

```
ENV app_name example
```

`docker-compose.yml` の 9 行目、 21 行目を編集する

9 行目
```
    - .:/start_project
```

↑ を変更する

```
    - .:/example
```

21 行目

```
  image: startproject_web
```

↑ を変更する

```
  image: example_web
```

## サーバ構築、 bundle install
下記のコマンドで、サーバを作成し、Bundle インストール

```
$ docker-compose build
$ docker-compose run web bundle install
```

## Rails プロジェクトを作る

```
$ docker-compose run web bundle exec rails new . --force -B --database=postgresql -T -S --no-rc
```

## bundle install
Gemfile が生成され、上書きされるので、再度 Bundle インストール

```
$ docker-compose run web bundle install
```

## データベースの接続設定
`config/database.yml` を編集し、接続情報を環境変数で設定する

```
default: &default
  adapter: postgresql
  encoding: unicode
  # For details on connection pooling, see Rails configuration guide
  # http://guides.rubyonrails.org/configuring.html#database-pooling
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
  username: <%= ENV['DB_USERNAME'] %>
  password: <%= ENV['DB_PASSWORD'] %>
  host: <%= ENV['DB_HOST'] %>
```

## データベースを作成する

DB の生成、マイグレーションを行う

```
$ docker-compose run web bundle exec rails db:create db:migrate db:seed
```

## 起動確認

サーバを起動する

```
$ docker-compose up
```

サーバが起動したら

http://localhost:3000/

へアクセスして、起動を確認する

## github へ push する
github にリポジトリを作成し、 push する

```
$ git remote set-url origin git@github.com:gupipo/xxxxxxxxxx.git
$ git add .
$ git commit -m 'new project'
$ git push -u origin master
```
