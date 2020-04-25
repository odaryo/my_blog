---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Nuxt.js+TypescriptのソースがIntelliJ IDEAで「Ts1219」Warningがでる"
date: 2020-03-05T23:31:54+09:00
featured: false
draft: false

# post thumb
image: "images/post/post_nuxt.svg"

# archives
archives:
  - "2020"
  - "2020/01"

# meta description
description: "this is meta description"

# taxonomies
categories:
  - "Nuxt.js"
tags:
  - "Nuxt.js"
  - "Typescript"
  - "JetBrains"

# post type
type: "post"
---


Nuxt.jsにTypescriptを適用して、Classを書こうとしたときに、下記エラーが出ました。

![warning](img-01.png)

```bash
TS1219: Experimental support for decorators is a feature that is subject to change in a future release. Set the 'experimentalDecorators' option in your 'tsconfig' or 'jsconfig' to remove this warning.
```

エディタはIntelliJ IDEAです。

メッセージの下にも出ていましたが、configファイルで「experimentalDecorators」を有効にすると良いらしい。

ということで、tsconfig.jsonの```compilerOptions```以下に下記記述を追加するとWarningが消えます。


```bash
{
  "compilerOptions": {
    ...
+   "experimentalDecorators": true,
    ...
```
