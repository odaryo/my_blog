---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Dockerでファイルのパーミッションをホストユーザと合わせる方法"
subtitle: ""
date: 2020-07-10T22:23:54+09:00
lastmod: 2020-07-10T22:23:54+09:00
draft: false

# post thumb
image: "images/post/post_docker.svg"

# meta description
description: ""

# taxonomies
categories: 
  - "開発環境"
  - "Docker"

tags:
  - "Docker"
  - "WSL2"
  - "Linux"

archives:
  - "2020"
  - "2020/07"

# post type (post, featured)
type: "post"
---

## はじめに

最近WSL2にDockerをインストールして、WSL2内でコーディングをするような環境を構築しました。  
その環境でDocker経由でファイルを作成するとパーミッションエラーが起こったためその解決方法をまとめます。

※WSL2にインストールしたDockerにて動作確認はしておりますが、Docker for WSL2では未検証です。

## 原因

Linux系のホストからDockerにボリュームをマウントするとホスト側から編集できなくなることがあります。  
これはホストのユーザとDockerコンテナ内のユーザIDが異なるためパーミッションエラーとなるためです。  

対処方法は、ホストとコンテナ内のユーザIDを合わせることです。

## 解決策

### /etc/passwdと/etc/groupをDockerコンテナにマウントする

やり方は下記の記事を参考にしています。

[「dockerでvolumeをマウントしたときのファイルのowner問題」](https://qiita.com/yohm/items/047b2e68d008ebb0f001)

docker-compose.ymlで/etc/passwdと/etc/groupをマウント  
コンテナ内で勝手に書き換えないようにro（read only）オプションをつけています。

```yaml
version: '3'

services:
  nginx:
    build:
      context: ./
      dockerfile: ./docker/nginx/Dockerfile
    ports:
      - 8080:80
    volumes:
      - ./app/public:/app/public:cached

  php-fpm:
    build:
      context: ./
      dockerfile: ./docker/php-fpm/Dockerfile
    volumes:
      - ./app:/app:cached
      - /etc/passwd:/etc/passwd:ro
      - /etc/group:/etc/group:ro
```

※Dockerfileの詳細は割愛します。

### 起動コマンド

コンテナを実行するときにユーザIDを指定してやることで、そのユーザと同じUIDでコンテナを操作できます。


```bash
# コンテナ起動済みの場合の例
$ docker-compose exec -u $(id -u $USER) nginx /bin/bash

# コンテナ未起動の場合
$ docker-compose run --rm -u $(id -u $USER) nginx /bin/bash
```

コマンドが長いので、.bashrcなどでエイリアスを設定しておくと良いかも。

```bash
$ cat .bashrc

...
alias doce='docker-compose exec -u $(id -u $USER)'
alias docr='docker-compose run --rm -u $(id -u $USER)'
...
```
## まとめ

Windows側のエディタでファイルを編集する場合はWSL側のパーミッションは無視できますが、  
WSL2側でファイル操作しようと思うとパーミッションも考慮する必要が出てきます。

気を付けよう。
