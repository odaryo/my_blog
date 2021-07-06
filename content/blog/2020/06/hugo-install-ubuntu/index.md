---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "UbuntuへHugoをインストール"
subtitle: ""
date: 2020-06-25T16:15:43+09:00
lastmod: 2020-06-25T16:15:43+09:00
draft: false

# post thumb
image: "images/post/post_hugo.svg"

# meta description
description: "UbuntuへHugoをインストールする手順を説明します"

# taxonomies
categories: 
  - "Hugo"

tags:
  - "Hugo"
  - "Ubuntu"

archives:
  - "2020"
  - "2020/06"

# post type (post, featured)
type: "post"
---

# はじめに

公式のページに情報はありますが、Linux版はHomuBrewでの方法しか書かれていなかったため、バイナリからインストールしました。

- [Install Hugo](https://gohugo.io/getting-started/installing/)

# インストール手順

下記リンク先より、Hugoの最新バイナリを取得します。

- https://github.com/gohugoio/hugo/releases

scssのコンパイルもしたかったので、extendedバージョンをダウンロードします。（2020/06/25時点）

```
$ wget hugo_extended_0.73.0_Linux-64bit.deb
```

Ubuntuではdebパッケージもaptでインストールできるので、管理しやすいようにaptで行います。

```
$ sudo apt install hugo_extended_0.73.0_Linux-64bit.deb
```

インストールされたことを確認する

```
$ hugo version
Hugo Static Site Generator v0.73.0-428907CC/extended linux/amd64 BuildDate: 2020-06-23T16:40:09Z
```

以上
