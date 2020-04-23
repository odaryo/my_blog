---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "docker環境におけるコマンドの省略方法"
archives: ["2019","2019/12"]
date: 2019-12-20T20:00:12+09:00
featured: false
draft: false

# post thumb
image: "images/post/post_docker.svg"

# meta description
description: "this is meta description"

# taxonomies
categories: 
  - "開発環境"
tags:
  - "Docker"

# post type
type: "post"
---

# はじめに

今のプロジェクトではDocker Composeを使って環境構築しています。

その時に問題なのが、コンテナ起動やコンテナ内で実行（composerとかartisanとか）したいときに打つコマンドの長さです。

毎回毎回```docker-compose exec app php artisan ...```など打つのはちょっと面倒。

コマンドのエイリアスを設定しても良いのですが、複数人で開発するときに全員に設定してもらうのもなぁ。。。   
との思いで、スクリプトファイルを用意しましています。

下記記事を参考にしています。感謝  
[Docker コマンドのショートカットをプロジェクトルートに置いたら快適になった - Qiita](https://qiita.com/acro5piano/items/740ea0726be6a745333d:title)

# ファイル構成

Linux（Bash）とWindows（PowerShell）の両方に対応できるよう作成しています。

```
┬－ bin      （スクリプトディレクトリ）
│    ├－ doc.sh
│    ├－ artisan.sh
│    ├－ composer.sh
│    ├－ doc.ps1
│    ├－ artisan.ps1
│    └－ composer.ps1
├－ docker （Dockerの設定）
│    └－ Dockerfile
├－ src  （ソースディレクトリ）
└－ docker-compose.yml
```

## bash用

- bin/doc.sh

```
#!/bin/bash

cmd="docker-compose $@"

echo $cmd
$cmd
```

- bin/artisan.sh  
コンテナ起動時はexec、停止時はrunコマンドで実行するように設定

```
#!/bin/bash

set -eu

if docker-compose ps app | grep -q Up; then
    cmd="docker-compose exec app php artisan $@"
else
    cmd="docker-compose run --rm app php artisan $@"
fi

echo $cmd
$cmd
```

- bin/composer.sh  

```
#!/bin/bash

set -eu

if docker-compose ps app | grep -q Up; then
    cmd="docker-compose exec app composer $@"
else
    cmd="docker-compose run --rm app composer $@"
fi

echo $cmd
$cmd
```

## powershell用

- bin/doc.ps1

```
$cmd = "docker-compose $Args"

echo $cmd
invoke-expression $cmd
```

- bin/artisan.ps1

```
$run = docker-compose.exe ps app | Out-String -Stream | Select-String "Up"

if($run) {
  $cmd = "docker-compose exec app php artisan $Args"
} else {
  $cmd = "docker-compose run --rm app php artisan $Args"
}

echo $cmd
invoke-expression $cmd
```

- bin/composer.ps1  
※artisan.ps1とほぼ同じため省略

# 使い方

プロジェクトルートに移動して下記コマンドを実行

```
# コンテナ起動
./bin/doc.ps1 up -d

# artisanコマンド実行
./bin/artisan.ps1 make:controller HogeController
```
