---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Node.jsのDockerコンテナにyarnをインストールしようとしてエラーが出た"
date: 2020-01-08T21:16:31+09:00
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
  - "Node.js"
  - "Docker"

# post type
type: "post"
---

## はじめに

DockerfileからNodeコンテナをビルドする際に下記エラーが起きました。

```bash
npm ERR! code EEXIST
npm ERR! syscall symlink
npm ERR! path ../lib/node_modules/yarn/bin/yarn.js
npm ERR! dest /usr/local/bin/yarn
npm ERR! errno -17
npm ERR! EEXIST: file already exists, symlink '../lib/node_modules/yarn/bin/yarn.js' -> '/usr/local/bin/yarn'
npm ERR! File exists: /usr/local/bin/yarn
npm ERR! Remove the existing file and try again, or run npm
npm ERR! with --force to overwrite files recklessly.
```

Dockerfileの内容

```bash
FROM node:12-alpine

RUN npm install -g yarn
```

## 原因と対策

Node.jsコンテナにはyarnがインストール済みなので、再度入れようとしたときにエラーとなります。

なので、yarnインストールの行を削除。  
無事起動完了。

そりゃ2重に入れようとしたらエラーとなるよな。。。  

nodeコンテナのissueを調べると、2017年にはyarnがインストール済みの模様

[Unable to npm install -g yarn on 6.10 · Issue #344 · nodejs/docker-node · GitHub](https://github.com/nodejs/docker-node/issues/344)

ちなみに、Node.jsのバージョンを「10系」から「12系」に上げたときに起きました。

バージョン上げる前もエラーが起きる要素はあったのですが、いままで起動できていたのが不思議。
