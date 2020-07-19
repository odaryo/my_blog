---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Windows版Gitでパーミッションを無視する方法"
date: 2019-12-02T20:49:45+09:00
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


同じリポジトリをWindows、Linux、Macなど別々の環境での開発を余儀なくされる場合、パーミッションの違いが差分扱いとなることがあります。
例えばLinuxでパーミッション```chmod 755 ```のように設定したファイルでも、Windows側では```644```として判定されるなど。

Windowsでも、ホストとWSL2で扱いが変わったりします。

これはWindowsにパーミッションの概念がないことが原因です。

解決策として、Windowsではパーミッションのチェックをしない設定があります。

WindowsでGit bashなどで実行

```bash
git config -g core.filemode false
```

※ 検索すると```-g```オプションがなくても設定できる記載があったのですが、私の環境ではできませんでした。


## ただし...

このオプションはすべての環境で行う必要があるそうです。
複数環境で開発している場合は、すべてでcore.filemode falseに設定する必要があります。

