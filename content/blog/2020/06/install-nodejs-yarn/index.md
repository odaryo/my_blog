---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "nvmを使ったYarnのインストール方法"
subtitle: ""
date: 2020-06-24T20:52:41+09:00
lastmod: 2020-06-24T20:52:41+09:00
draft: false

# post thumb
image: "images/post/post_linux.svg"

# meta description
description: ""

# taxonomies
categories: 
  - "Node.js"

tags:
  - "Node.js"

archives:
  - "2020"
  - "2020/06"

# post type (post, featured)
type: "post"
---

## はじめに

WSL2のUbuntu環境にnvm経由でyarnをインストールした際に、最新バージョンがインストールされなかったので手順をメモします。  

管理の簡略化や、異なるバージョンを切り替えることもあるためnvmからインストールしました。

## nvmのインストール

はじめにnvmをインストールしてやります。

nvmはNode Version Managerの略。nodeのバージョン管理ツールです。  
プロジェクトによっては、nodeのバージョンが違うと動かないので切り替えツールがあると便利です。

インストール方法

```bash
$ curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.0/install.sh | bash
$ source $HOME/.bashrc
```

## Node.js（LTS）のインストール

nvmを使ってnode.jsをインストール

ここでは--ltsオプションを指定して、LTS版をインストールします。

```bash
$ nvm install --lts
```

LTSの最新版がインストールされたことを確認します。

```bash
$ node -v
v12.18.1

$ npm -v
6.14.5
```

## yarnをインストール

npmからインストールするだけだと意図しないものが参照されるようです。  
（nodeのバージョンに沿ったものではなく、OS本体に入っているyarnが参照される）

そこで、yarnのインストール前にnvmコマンドでnodeのバージョンを指定します。

```bash
$ nvm use 12.18.1
```

その後インストール

```bash
$ npm install -g yarn
```

最新版がインストールされたことを確認

```bash
$ yarn -v
1.21.1
```

