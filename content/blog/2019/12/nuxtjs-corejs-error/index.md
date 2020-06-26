---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "nuxt.jsのbuildでcore-jsのエラーが出る"
date: 2019-12-10T15:45:59+09:00
lastmod: 2020-06-26T15:52:41+09:00
featured: false
draft: false

# post thumb
image: "images/post/post_nuxt.svg"

# archives
archives:
  - "2019"
  - "2019/12"

# meta description
description: "this is meta description"

# taxonomies
categories: 
  - "Nuxt.js"
tags:
  - "Nuxt.js"
  - "Vue.js"

# post type
type: "post"
---

※ issueを再確認すると別の解法のレスがあったので追記します。（2020/06/26追記）

# エラーの内容

Nuxt.js + Vuetifyの環境でbuildすると、下記のようなエラーが出るようになりました。

```
Module not found: Error: Can't resolve 'core-js/modules/...' in '/app/...'
```
buildログを一部抜粋

```bash
$ yarn build

...

ERROR in ./node_modules/vuetify/lib/components/VTabs/VTabs.js
Module not found: Error: Can't resolve 'core-js/modules/web.dom.iterable' in '/app/node_modules/vuetify/lib/components/VTabs'
 @ ./node_modules/vuetify/lib/components/VTabs/VTabs.js 3:0-42
 @ ./node_modules/vuetify/lib/components/VTabs/index.js
 @ ./node_modules/vuetify/lib/components/index.js
 @ ./node_modules/vuetify/lib/index.js
 @ ./.nuxt/vuetify/plugin.js
 @ ./.nuxt/index.js
 @ ./.nuxt/client.js
 @ multi ./.nuxt/client.js

ERROR in ./node_modules/vuetify/lib/components/VTabs/VTabsBar.js
Module not found: Error: Can't resolve 'core-js/modules/web.dom.iterable' in '/app/node_modules/vuetify/lib/components/VTabs'
 @ ./node_modules/vuetify/lib/components/VTabs/VTabsBar.js 6:0-42
 @ ./node_modules/vuetify/lib/components/VTabs/VTabs.js
 @ ./node_modules/vuetify/lib/components/VTabs/index.js
 @ ./node_modules/vuetify/lib/components/index.js
 @ ./node_modules/vuetify/lib/index.js
 @ ./.nuxt/vuetify/plugin.js
 @ ./.nuxt/index.js
 @ ./.nuxt/client.js
 @ multi ./.nuxt/client.js

ERROR in ./pages/login.vue?vue&type=script&lang=js& (./node_modules/babel-loader/lib??ref--2-0!./node_modules/vuetify-loader/lib/loader.js??ref--16-0!./node_modules/vue-loader/lib??vue-loader-options!./pages/login.vue?vue&type=script&lang=js&)
Module not found: Error: Can't resolve 'core-js/modules/web.dom.iterable' in '/app/pages'
 @ ./pages/login.vue?vue&type=script&lang=js& (./node_modules/babel-loader/lib??ref--2-0!./node_modules/vuetify-loader/lib/loader.js??ref--16-0!./node_modules/vue-loader/lib??vue-loader-options!./pages/login.vue?vue&type=script&lang=js&) 3:0-42
 @ ./pages/login.vue?vue&type=script&lang=js&
 @ ./pages/login.vue
 @ ./.nuxt/router.js
 @ ./.nuxt/index.js
 @ ./.nuxt/client.js
 @ multi ./.nuxt/client.js

 FATAL  Nuxt build error                                                                                                                                                                                                                                                        06:14:11

  at WebpackBundler.webpackCompile (node_modules/@nuxt/webpack/dist/webpack.js:5314:21)
  at processTicksAndRejections (internal/process/task_queues.js:93:5)
  at async WebpackBundler.build (node_modules/@nuxt/webpack/dist/webpack.js:5263:5)
  at async Builder.build (node_modules/@nuxt/builder/dist/builder.js:5597:5)
  at async Generator.initiate (node_modules/@nuxt/generator/dist/generator.js:70:7)
  at async Generator.generate (node_modules/@nuxt/generator/dist/generator.js:40:5)
  at async Object.run (node_modules/@nuxt/cli/dist/cli-build.js:96:7)
  at async NuxtCommand.run (node_modules/@nuxt/cli/dist/cli-command.js:2575:7)


   ╭─────────────────────────────╮
   │                             │
   │   ✖ Nuxt Fatal Error        │
   │                             │
   │   Error: Nuxt build error   │
   │                             │
   ╰─────────────────────────────╯

error Command failed with exit code 1.
```

# 対策

## 対策①
モジュールを最新にしても解決せず。

調べているとこんなissueが上がっていました。

https://github.com/nuxt/nuxt.js/issues/5287

core-jsが3.xになるとエラーが起きるらしいです。


2系の最新版(2019/12/10時点)をインストールするように明記

```bash
$ yarn add core-js@2.6.11
```

```bash
$ yarn build
```

成功！

~~ビルドするとcore-jsを3系にしろとwarningが出るようになったので対策を考えないと。。。~~


▽=== 2020/06/25追記 ===▽

## 対策②

https://github.com/nuxt/nuxt.js/issues/5287#issuecomment-622660147 に別の解法が載っていました。  
こちらだとcore-js 3.xにあげても問題なく動作します。


nuxt.config.jsに下記を追加


```javascript
export default {
  build: {
    babel: {
      presets({ isServer }) {
        return [
          [
            require.resolve('@nuxt/babel-preset-app'),
            // require.resolve('@nuxt/babel-preset-app-edge'), // For nuxt-edge users
            {
              buildTarget: isServer ? 'server' : 'client',
              corejs: { version: 3 }
            }
          ]
        ]
      }
    }
  }
}
```

package.jsonに「core-js」を追加していたら、記述を削除  
（package.jsonに明示的に書かなくても問題ありません。）

下記の依存関係の確認でも古いバージョンを使っていると怒られなくなりました。

```bash
$ yarn outdated
```

なお、このやり方はNuxt.js v2.6.0時点のリリースノートに書かれているようです。

△=== 2020/06/25追記 ===△

