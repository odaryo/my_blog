---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "snapでインストールしたhugoで'permission denied'エラー"
date: 2020-01-25T01:12:53+09:00
featured: false
draft: false

# post thumb
image: "images/post/post_hugo.svg"

# archives
archives:
  - "2020"
  - "2020/01"

# meta description
description: "this is meta description"

# taxonomies
categories:
  - "Go"
tags:
  - "Go"
  - "Hugo"
  - "Linux"

# post type
type: "post"
---

# はじめに

HugoはGO言語で作られた静的サイトジェネレーターです。  
この度ブログを作成しようとHugoをインストールしたところ、プロジェクト作成時に下記エラーに見舞われました。

```bash
$ hugo new site myblog
Error: Failed to create dir: mkdir /var/lib/snapd/void/myblog: permission denied
```


# 結論

公式のドキュメントに書いてあったのですが、  
snapでインストールしたhugoコマンドは、$HOMEか、gvfsでマウントしたディレクトリ以外に書き込めないとのこと。

ホームディレクトリで作成コマンドを実行すると、問題なく作成できしました。

- 参照: https://gohugo.io/getting-started/installing/#snap-package


# 環境


- OS: Ubuntu 18.04

```
$ cat /etc/lsb-release 
DISTRIB_ID=Ubuntu
DISTRIB_RELEASE=18.04
DISTRIB_CODENAME=bionic
DISTRIB_DESCRIPTION="Ubuntu 18.04.3 LTS"
```

- Snap

```
$ snap version
snap    2.42.5
snapd   2.42.5
series  16
ubuntu  18.04
kernel  5.0.0-37-generic
```

- Hugo

```
$ which hugo
/snap/bin/hugo

$ hugo version
Hugo Static Site Generator v0.63.1/extended linux/amd64 BuildDate: 2020-01-24T01:20:28Z
```

Hugoのインストール時のコマンド


```
$ sudo snap install hugo --channel=extended 
```

# まとめ

公式の文章はよく読みましょう。
