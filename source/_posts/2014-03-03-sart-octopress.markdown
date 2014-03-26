---
layout: post
title: "Octopress にブログ乗り換えました"
date: 2014-03-03 10:41:55 +0900
comments: true
categories: News
---

# きっかけ - 静的HTMLはえー

今まで [スマートWP][1] や [Smart Designing][2]  などをリリースしてきて、Webホスティングクラウドを運用してみて思ったことは、静的HTMLファイルのWebサーバのパフォーマンスは圧倒的に低負荷にできるなぁというところだった。

Nginx + 静的HTML on AWS EC2 micro でパフォーマンステストするとサーバのCPUは0.2−0.3%程度で、先にネットワーク帯域が足りなくてエラーが起きる。
静的HTMLのWebホスティングにかかるサーバのランニングコストは最小限にできることがわかった。

逆に Wordpress とか Rails など php/ruby が HTML を生成する場合は、php が走っている間はCPUがMaxになるしメモリも使う。micro で毎回ページ生成させると CPU 使用量の上限に達して steal 発生しまくりでとてもじゃないけど運用できない。

# 原点回帰の流れ

古いけど静的HTMLでWebサイト運用できれば最もコストが低く、最もパフォーマンスの良い環境が作れる。

原点回帰というかそんな流れを感じていた今日このごろ、[Octopress][3] を知った。

Markdown で書いてローカルで静的HTMLをジェネレートしてサーバにデプロイ。
テーマはシンプルで個人的には好きだし、これでいいじゃん。ということで今後のブログの新しいポストは Octopress で作ったこのブログにしていきます。

ちなみにこのブログは [Smart Designing][2] 上で動いていて、デプロイ先をローカルの Dropbox フォルダに変えて実現しています。

この方法は後々紹介できればです。


では、新しいブログをよろしくお願いします。


[1]: http://www.shakesoul.net/smartwp
[2]: http://smartdesigning.me/
[3]: http://octopress.org/