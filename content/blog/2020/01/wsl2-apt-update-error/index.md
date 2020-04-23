---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "WSL2のapt updateでエラーが出る"
date: 2020-01-15T21:04:32+09:00
featured: false
draft: false

# post thumb
image: "images/post/post_laravelvue.svg"

# archives
archives:
  - "2020"
  - "2020/01"

# meta description
description: "this is meta description"

# taxonomies
categories:
  - "WSL2"
tags:
  - "WSL2"
  - "Windows"

# post type
type: "post"
---

WSl2にUbuntu 18.04を入れたところ、```apt update```コマンドで下記のようなエラーが出るようになりました。

```
$ sudo apt update
Hit:1 https://download.docker.com/linux/ubuntu bionic InRelease
Hit:2 http://ppa.launchpad.net/fish-shell/release-3/ubuntu bionic InRelease
Get:3 http://security.ubuntu.com/ubuntu bionic-security InRelease [88.7 kB]
Hit:4 http://archive.ubuntu.com/ubuntu bionic InRelease
Get:5 http://archive.ubuntu.com/ubuntu bionic-updates InRelease [88.7 kB]
Get:6 http://archive.ubuntu.com/ubuntu bionic-backports InRelease [74.6 kB]
Reading package lists... Done
E: Release file for http://security.ubuntu.com/ubuntu/dists/bionic-security/InRelease is not valid yet (invalid for another 1d 5h 5min 35s). Updates for this repository will not be applied.
E: Release file for http://archive.ubuntu.com/ubuntu/dists/bionic-updates/InRelease is not valid yet (invalid for another 1d 5h 6min 25s). Updates for this repository will not be applied.
E: Release file for http://archive.ubuntu.com/ubuntu/dists/bionic-backports/InRelease is not valid yet (invalid for another 1d 5h 7min 36s). Updates for this repository will not be applied.
```

## 結論

WSL2の時刻がずれていたのが問題でした。

## 環境

- ホスト
    - Windows10 Professional
        - バージョン2004（OSビルド 19041.1）
- WSL2
   - Ubuntu 18.04

## 原因と解決方法

エラーメッセージを調べると、どうやら時刻がずれているときに起こる模様。

確かに、ログに「invalid for another 1d 5h 5min 35s」と書いてます。

dateコマンドで確認したところ、1日と5時間ほど遅れている。。。  
タイムゾーンもJSTで設定には問題なさそうなので、  
手っ取り早く再起動。

コマンドプロンプトを管理者権限で開いて下記コマンドを実行するか、

```
net stop LxssManager
net start LxssManager
```

PowerShellで下記コマンドを実行する。

```
wslconfig /t Ubuntu-18.04
```

無事更新完了

## 終わりに

昨年のアップデートでスリープ復帰時に時刻を同期する更新が入っているので、スリープ→復帰では問題なさそう。

参考：[WSL その184 - Build 18970のWSLに関する変更点・Linux kernelのアップデート](https://kledgeb.blogspot.com/2019/09/wsl-184-build-18970wsllinux-kernel.html)

今回の件はスリープにして何日か放置していたのが原因だろうか？

