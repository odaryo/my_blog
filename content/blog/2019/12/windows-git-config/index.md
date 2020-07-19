---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Windows開発環境でのgitの設定"
date: 2019-12-02T20:35:21+09:00
featured: false
draft: false

# post thumb
image: "images/post/post_git.svg"

# archives
archives:
  - "2019"
  - "2019/12"

# meta description
description: ""

# taxonomies
categories: 
  - "開発環境"
tags:
  - "Windows"
  - "Git"

# post type
type: "post"
---


Windows環境でのGitはGit for Windowsを使っています。

Windowsで開発するときに気をつけないといけないものはいくつかありますが、そのうちの一つが改行コードの設定です。

デフォルトのままGit for Windowsを利用すると、```git pull```するときに改行コードがCRLFでアップされたりします。

それを防ぐために下記を設定します。

```bash
git config -g core.autocrlf input
```

autocrlfのオプションの意味はこちら

|設定|チェックアウト時|コミット時|
|---|---|---|
|true|LF→CRLF|CRLF→LF|
|input|変換しない|CRLF→LF|
|false|変換しない|変換しない|


Windowsでは「input」か「false」にするべきですが、新規ファイル作成時にCRLFになる可能性があるので私は「input」を選択しています。
