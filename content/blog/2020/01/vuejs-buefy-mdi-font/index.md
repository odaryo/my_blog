---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Vue.js + BuefyでMdi Fontが表示されない"
date: 2020-02-04T19:37:28+09:00
featured: false
draft: false

# post thumb
image: "images/post/post_vue.svg"

# archives
archives:
  - "2020"
  - "2020/01"

# meta description
description: ""

# taxonomies
categories:
  - "Vue.js"
tags:
  - "Vue.js"
  - "Buefy"

# post type
type: "post"
---

Vue.js + Buefyでプロジェクトを作った際、Material Design Iconsがうまく表示されなかったため、解決方法をメモ。

mdiフォントのインストール

```bash
$ npm i @mdi/font
or
$ yarn add @mdi/font
```

main.jsに下記を追加

```bash
import "@mdi/font/css/materialdesignicons.css";

Vue.use(Buefy);
```
以上


参考: https://kalappo.net/storybook-buefy-font/
