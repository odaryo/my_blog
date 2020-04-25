---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Vue.js + Laravelの環境をDockerを使って作ってみる"
date: 2020-01-30T12:04:37+09:00
featured: false
draft: true

# post thumb
image: "images/post/post_laravelvue.svg"

# archives
archives:
  - "2020"
  - "2020/01"

# meta description
description: "this is meta description"

# taxonomies
categories:
  - "開発環境"
tags:
  - "Vue.js"
  - "Laravel"
  - "Docker"

# post type
type: "post"
---


## 概要

Docker Composeを利用したVue.js (フロント) + Laravel (API) の環境構築についてまとめます。

フロントからバックエンドにAPIアクセスする際に、ドメインが異なるとCORSなどを設定する必要があるため、同一ドメイン内に構築する方法で設定します。

Vue.jsはVue CLIでプロジェクトを作成し、TypescriptとBuefyを適用させています。


## 環境

- ホスト
    - Ubuntu 18.04
    - Docker: version 19.03.5, build 633a0ea838
    - Docker Compose: version 1.25.0, build 0a186604
- ツール
    - Vue.js
        - Vue-CLI 4.1.2
        - Typescript 3.5.0
        - Buefy 0.8.0
    - Laravel 6.x


## ディレクトリ構造

```bash
+ api/ # Laravel directory
+ docker/ # dockerfiles directory
    + api/
    + db/
    + front/
    + nginx-proxy/
+ front/ # Vue.js directory
+ docker-compose.yml
+ prod.docker-compose.yml
```

## 開発環境での起動方法

開発環境ではSSL化は想定していません。

### 起動コマンド

```bash
$ docker-compose build
$ docker-compose up -d
```

### アクセスURL

- フロントエンド
    - localhost:8080
- バックエンド
    - localhost:8080/api/

### 初回のインストール方法

#### 1. Vue.js

#### Vue.jsプロジェクトの作成

Vue.jsはVue-CLIでインストールします。
<project_dir>/frontの中身が空の場合はfrontコンテナが立ち上がらないため、runで起動します。

```bash
$ cd <project_dir>

$ doc run --rm front vue create .

?  Your connection to the default yarn registry seems to be slow.
   Use https://registry.npm.taobao.org for faster installation? Yes

Vue CLI v4.1.2
? Generate project in current directory? Yes

Vue CLI v4.1.2
? Please pick a preset: Manually select features
? Check the features needed for your project: Babel, TS, PWA, Router, Vuex, CSS Pre-processors, Linter, Unit
? Use class-style component syntax? Yes
? Use Babel alongside TypeScript (required for modern mode, auto-detected polyfills, transpiling JSX)? Yes
? Use history mode for router? (Requires proper server setup for index fallback in production) Yes
? Pick a CSS pre-processor (PostCSS, Autoprefixer and CSS Modules are supported by default): Sass/SCSS (with dart-sass)
? Pick a linter / formatter config: Prettier
? Pick additional lint features: Lint on save, Lint and fix on commit (requires Git)
? Pick a unit testing solution: Jest
? Where do you prefer placing config for Babel, ESLint, etc.? In dedicated config files
? Save this as a preset for future projects? No
? Pick the package manager to use when installing dependencies: Yarn
```

#### Vue.jsの起動確認

Vue.js単体での動作確認をします。
（実際の開発時はdocker-compose upで```yarn serve```が実行されるようになっています）

```bash
$ cd front
$ yarn serve
```

localhost:8080にアクセスして表示されればOK


#### 追加パッケージをインストール

AxiosとBuefyをインストールします。

```bash
$ pwd
<project_dir>/front
$ yarn add axios buefy
```

#### 2. Laravel 6.0

```bash
$ pwd
<projct_dir>

# 初回に作成する場合は、apiディレクトリの中身を空にして実行すること
$ rm -r api/public

# Laravel最新版をインストール(末尾に.をつける)
$ docker-compose exec php-fpm composer create-project --prefer-dist laravel/laravel .

# redisを利用する設定
$ docker-compose exec php-fpm composer require predis/predis
```

#### 3.docker再起動

```bash
$ docker-compose down
$ docker-compose up -d
```

## 本番環境での起動方法

### ネットワーク作成

https-proxyコンテナでSSL化とプロキシ設定を行うため、共通ネットワークを作成しておく

[https-proxyのサンプル](https://github.com/odaryo/docker_ssl_proxy)

```bash
$ docker network create --driver bridge proxy_network
```

### 起動コマンド

```bash
$ docker-compose -f prod.docker-compose.yml build
$ docker-compose -f prod.docker-compose.yml up -d
```
