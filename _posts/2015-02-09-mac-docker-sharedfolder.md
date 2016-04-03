---
layout: post
title: "Mac - boot2docker 1.4.1 間共有フォルダ設定方法"
date: 2015-02-09 19:00:00 +0900
comments: true
categories: 
  - Tech
---
{% img /images/2015/02/20150209-docker.png 'docker' %}

Mac で [Docker][] を動かそうとすると [boot2docker][] は必須。

Vagrant みたいに開発環境用サーバとして Docker を扱うには、共有フォルダの設定を自分でやらなくてはならない。

自分的には Dockerfile も Mac のローカルフォルダで管理したいから、Mac - boot2docker 間で共有フォルダを設定して、

1. Mac 上で Dockerfile を作る
1. boot2docker ssh
1. docker build -t centos:rails

みたいにやりたい。

もちろん、Dcoker container に Mac でコーディングしたソースは自動的にデプロイしたいので、boot2docker - docker container 間も共有フォルダにする必要はあるのだけど、まずは Mac - boot2docker 間を共有したい。

調べてみたが検索に引っかかるのは古い情報で、Github の README.md に記載を見つけて動いた。

2015.02.09現在 boot2docker 1.4.1 で動かすための方法をメモしておく。

# 情報のソース

[boot2docker の github](https://github.com/boot2docker/boot2docker) の README.md 内の **VirtualBox Guest Additions**

https://github.com/boot2docker/boot2docker#virtualbox-guest-additions

> The first of the following share names that exists (if any) will be automatically mounted at the location specified:
>
> 1. Users share at /Users
> 1. /Users share at /Users
> 1. c/Users share at /c/Users
> 1. /c/Users share at /c/Users
> 1. c:/Users share at /c/Users

という記述がある。

<!-- more -->

# 古い boot2docker が入っている人

実行前には、`$ boot2docker stop` して boot2docker VM を停止しておく。

boo2docker のアップグレードをします。Mac のターミナル上で、

`$ boot2docker upgrade`

を実行。

すると、新しい iso ファイルが、`$HOME/.boot2docker/boot2docker.iso`(自分の場合は、`/Users/hironobu/.boot2docker/boot2docker.iso`) にダウンロードされます。

確認したければ、`$ boot2docker up && boot2docker ssh` してログインプロンプトに `Boot2Docker version 1.4.1` が見えればOK。

# VirtualBox コマンドで共有する

VirtualBox の Share Name を `Users` にすればうまくマウントしてくれそう。以下を Mac のターミナル上で実行する。

`
$ VBoxManage sharedfolder add boot2docker-vm --name Users --hostpath $HOME
`

`boot2docker-vm` は VirtualBox 上の boot2docker の VM 名。
`$HOME` は自分の場合、 `/Users/hironobu` になる。

VirtualBox 上の設定画面は以下のようになった。

{% img /images/2015/02/20150209-ScreenShot.png 'VirualBox screen shot %}

古い情報だと Share Name を `home` にすると書いてあるブログもあったが、やってみたが認識しなかった。

# 確認

```bash
$ boot2docker up

$ boot2docker ssh
                        ##        .
                  ## ## ##       ==
               ## ## ## ##      ===
           /""""""""""""""""\___/ ===
      ~~~ {~~ ~~~~ ~~~ ~~~~ ~~ ~ /  ===- ~~~
           \______ o          __/
             \    \        __/
              \____\______/
 _                 _   ____     _            _
| |__   ___   ___ | |_|___ \ __| | ___   ___| | _____ _ __
| '_ \ / _ \ / _ \| __| __) / _` |/ _ \ / __| |/ / _ \ '__|
| |_) | (_) | (_) | |_ / __/ (_| | (_) | (__|   <  __/ |
|_.__/ \___/ \___/ \__|_____\__,_|\___/ \___|_|\_\___|_|
Boot2Docker version 1.4.1, build master : 86f7ec8 - Tue Dec 16 23:11:29 UTC 2014
Docker version 1.4.1, build 5bc2ff8

docker@boot2docker:~$ ll /
total 4
drwxr-xr-x    1 docker   staff         3332 Jan 27 23:34 Users/

(snip)

docker@boot2docker:~$ ll /Users/

(snip)
```

`/Users` が boot2docker 内に見えて、`/Users` 内を見ると Mac の $HOME(自分の場合は、`/Users/hironobu`)が見えるはず。




ということでした (・ω<)

快適な docker 開発ライフを〜


 [Docker]: https://www.docker.com/
 [boot2docker]: http://boot2docker.io/