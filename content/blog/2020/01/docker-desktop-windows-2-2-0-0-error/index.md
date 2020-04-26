---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Docker Desktop for Windows 2.2.0.0のdocker-composeでエラー"
date: 2020-01-28T11:08:09+09:00
featured: false
draft: false

# post thumb
image: "images/post/post_docker.svg"

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
  - "Docker"
  - "Windows"

# post type
type: "post"
---

Windows10のdocker desktop for window環境で、docker-composeで起動中のコンテナを操作しようとしたところ、下記エラーが出るようになりました。

```bash
> docker-compose exec web /bin/bash
Traceback (most recent call last):
  File "docker-compose", line 6, in <module>
  File "compose\cli\main.py", line 72, in main
  File "compose\cli\main.py", line 128, in perform_command
  File "compose\cli\main.py", line 491, in exec_command
  File "compose\cli\main.py", line 1469, in call_docker
  File "subprocess.py", line 172, in call
  File "subprocess.py", line 394, in __init__
  File "subprocess.py", line 644, in _execute_child
TypeError: environment can only contain strings
[16872] Failed to execute script docker-compose
```

エラーとなるのは起動後にコンテナ内のコマンドを実行しようとした場合で、  
コンテナ起動やapacheなどのサービスの動作は問題ないようです。

## 原因

docker-composeのバージョン 1.25.2 でエラーが起きる模様です。

別のバージョン（v1.24.1かv1.25.3）を使うことで解決するとの記載がありました。

参考： https://github.com/docker/compose/issues/7169

docker desktop for windows が更新（v2.2.0.0：2020/1/21リリース）された際にdocker-composeのバージョンも上がり、この問題が発生するようです。


## 環境

- Windows 10 Pro 1903 (18362.592)
- docker desktop for windows v2.2.0.0 (42247)
- Docker version 19.03.5, build 633a0ea
- docker-compose version 1.25.2, build 698e2846


## 解決方法

「docker-compose.exe」のバイナリを最新版に置き換えます。

<b>１．「docker-compose v1.25.3」をダウンロード</b>

下記ページの下のほうにある、「docker-compose-Windows-x86_64.exe」をダウンロードします。

https://github.com/docker/compose/releases/tag/1.25.3

<b>２．ダウンロードしたファイル名を「docker-compose.exe」にリネーム</b>

<b>３．「docker-compose.exe」を既存のものと置き換える</b>

リネームした「docker-compose.exe」を既存ファイルに上書きしてください。  
デフォルトでは下記フォルダにあるかと思います。

例：C:\Program Files\Docker\Docker\resources\bin\docker-compose.exe

※ 参照先のissueでは対策ファイルへのパスを上書き指定することで対応していますが、実体を上書きしても問題ないと判断し「docker-compose.exe」自信を上書きする手順に変更してあります。

<b>４．動作確認</b>

バージョンが上がっていることを確認します。

```bash
> docker-compose -v
docker-compose version 1.25.3, build d4d1b42b
```

無事実行できました。

```bash
> docker-compose exec web /bin/bash
root@7bf1597be011:/var/www/html# 
```

## 終わりに

ユーザ数の多いOSSだとメジャーなバグはすぐに報告が上がって対策されるので良いですね。

