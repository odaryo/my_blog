---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "nuxt generateでページが作成されない"
subtitle: ""
date: 2020-03-16T09:42:42+09:00
lastmod: 2020-03-16T09:42:42+09:00
draft: false

# post thumb
image: "images/post/post_nuxt.svg"

# meta description
description: "Nuxt.jsを使った静的環境で、nuxt generateをしてもページが作成されない場合の対応"

# taxonomies
categories:
  - "Nuxt.js"

tags:
  - "Nuxt.js"

archives:
  - "2020"
  - "2020/03"

# post type (post, featured)
type: "post"
---

## はじめに
Nuxt.jsで静的Webサイトを作成しようと```nuxt generate```したところ、作成されたページを開いても真っ白でローディングイメージのみ表示されている状態になりました。

![blank](img-01.png)

## 原因

いろいろググりながら試してたところ、原因は***SPAモード***に設定していたためでした。  
```nuxt generate```はSSRの機能を利用して静的ファイルを作成するようです。

```nuxt.config.js```のmodeを初期値に変更

```
module.exports = {
-  mode: 'spa',  // <- spaの設定を削除
  /*
  ** Headers of the page
  */
  head: {
  ...
```

SSRを使わないからSPAモードにしようとしたのが間違いでした。

ちなみに、プロジェクトを作る際の「server framework」はNoneでも問題ありませんでした。

```
*  Choose custom server framework (Use arrow keys)
> None (Recommended) 
  AdonisJs 
  Express 
  Fastify 
  Feathers 
  hapi 
  Koa 
  Micro 
```