---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "nuxt.jsのDocker開発環境"
date: 2019-12-05T10:43:38+09:00
featured: false
draft: false

# post thumb
image: "images/post/post_nuxt.svg"

# archives
archives:
  - "2019"
  - "2019/12"

# meta description
description: "this is meta description"

# taxonomies
categories: 
  - "開発環境"
tags:
  - "Nuxt.js"
  - "Docker"

# post type
type: "post"
---

Docker Composeを使ってNuxt.jsの開発環境を構築したときの手順をまとめます。

## 動作環境

- Ubuntu18.04

## ファイルの説明

### ファイル構成

```
┬ .env
├ docker-compose.yml
├ docker
│    └ Dockerfile
└ front  // ソースコード
```

### docker-compose.yml

```yml
version: '3'

services:

  # frontend
  front:
    build:
      context: ./
      dockerfile: ./docker/Dockerfile
      args:
        - NODE_VERSION=${NODE_VERSION}
        - USER_ID=${UID}
        - GROUP_ID=${GROUPS}
    environment:
      HOST: "0.0.0.0"
    command: bash -c "yarn && yarn dev"
    volumes:
      - ./front:/app:cached
```

### Dockerfile

デフォルトだとnpmやyarnがroot権限で動くため、モジュールやビルドファイルがrootで作成されてしまいます。
ファイル削除の際などに面倒なのでパーミッションをユーザと合わせます。

Dockerfileにuser_id/group_idの変更を記載します。

```bash
ARG NODE_VERSION

FROM node:${NODE_VERSION}-alpine

RUN apk update \
    && apk upgrade \
    && apk add --no-cache bash \
    && npm install -g nuxt

# change user
ARG USER_ID
ARG GROUP_ID

RUN deluser node && \
    addgroup -g ${GROUP_ID} -S node && \
    adduser -u ${USER_ID} -S node -G node

USER node:node

WORKDIR /app
```

### .env

.envにdocker-compose.yml, Dockerfileで使用する各パラメータを記載します

```bash
# Dockerプロジェクト名
COMPOSE_PROJECT_NAME=nuxt_sanple

# ホストのユーザIDと合わせて設定
UID=1000
GROUPS=1000

### Node ##################################################
NODE_VERSION=12
```

## 起動方法

```bash
$ docker-compose build (初回のみ)
$ docker-compose up -d
```

### 補足

nuxt.config.jsを編集した場合は自動読み込みされないため、下記コマンドでリスタートします。

```bash
$ docker-compose restart
```
