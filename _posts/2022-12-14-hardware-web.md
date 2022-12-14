---
layout: post
title: "ハードウエアのWeb化"
date: 2022-12-14 10:40:00 +0900
comments: true
categories:
  - Business
tags:
  - ハードウエア
  - Web化
  - IoT
description: ハードウエアの世界は縁遠いと思っていて、「IoTの時代だ」と言われても、積極的にアプローチしようとは思わなかったが、どうも最近のハードウエアはLinuxで動いているようで、「ほー、興味深いな」と一気に親近感を持った。そうしたらハードウエアのWeb化というキーワードが浮かんだ。
image_out: https://lh3.googleusercontent.com/pw/AL9nZEXL_TJmtsoQMQ6PgFwGvH_dYI16brXF9_-mGNalEiC9O6WN_Iso641bcVH2H8EkCbjqZAjajNQhiHTwjQSDuX4KgbQ8WhV4vTdlYpW4ZhPXoXYYrktuPWf_erdFjE76QZUBJXurzBYwDoa9GovcWfLrsQ=w1196-h673-no?authuser=0

---
![World](https://lh3.googleusercontent.com/pw/AL9nZEXL_TJmtsoQMQ6PgFwGvH_dYI16brXF9_-mGNalEiC9O6WN_Iso641bcVH2H8EkCbjqZAjajNQhiHTwjQSDuX4KgbQ8WhV4vTdlYpW4ZhPXoXYYrktuPWf_erdFjE76QZUBJXurzBYwDoa9GovcWfLrsQ=w1196-h673-no?authuser=0)

Web周りのテクニカルアドバイザーの仕事で、ハードウエア開発にちょこっとだけ絡んでいる。

ハードウエアの世界は縁遠いと思っていて、「IoTの時代だ」と言われても、積極的にアプローチしようとは思わなかったが、どうも最近のハードウエアはLinuxで動いているようで、「ほー、興味深いな」と一気に親近感を持った。

ちょっと調べてみると、「組み込みLinux」という単語もあるようで、ハードウエアのLinux化はすでに進んでいるみたい。

# 組み込みLinux

[IoTに最適化されたLinuxも出ているようだし](https://monoist.itmedia.co.jp/mn/articles/2210/31/news066.html)、[組み込みLinuxの実践的な記事も出ている](https://www.aps-web.jp/academy/18209/)。

すでに**ハードウエアのLinux化**は当然のアプローチみたい。

確かに、IPアドレスベースでの通信が可能だし、セキュリティ、ユーザー管理などもLinuxにおまかせできる。

IoTな機器だと、IPv6割り当てて、インターネットに繋がって、HTTPでWebAPIサーバーとの通信をすることもありだし、Linuxで動かすメリットは大きそう。

# Linux化するってことはUIはWeb化するのでは？

そうするとUIはブラウザーベースでも可能になる。

なんとなくだけど、ハードウエアのUIってスタティックで、決まったレイアウトのViewしか出せないイメージがある。

LinuxならWebサーバーを立てて、UIをWebブラウザーにしてし locahost 内でHTTP通信させれば、パソコン・スマホの世界と同じアプローチで行ける。
SPAで動的に見せることも可能。

構成は、
```
Javascript
Web Browser
    |
    | HTTP
    |
Web Server (API)
Docker Container
    |
    | HTTP?
    |
Data (from sensors)
```
みたいな感じで行けそう。

Docker にしておけば、手元での開発環境との整合性も担保できるし、Docker部分は開発ベンダーが担当みたいに、責任分界点もきれいに分けられる。

# 懸念は表示のスムーズさかな

懸念は、ブラウザーベースにするので、今までのスタティックなViewよりかは、マシンスペックが低ければ、アニメーションがカクカクしてしまうかもしれない。

やっぱり、ブラウザーの描画処理って重いし、基本的に表示内容がリッチになればそれだけ負荷が大きくなる。

IoTな機器はコスト削減のために、マシンスペックをなるべく低くしたいだろうから、ブラウザーで動的ページを常にゴリゴリ動かすようなことには耐えられないかもしれない。

まぁ、この懸念はハードウエアのスペックが上がってくれば払拭されるけど。

楽観的に、ムーアの法則へ期待して、物理で殴れば懸念も引っ込む感じで。


# 競合は Android, iOSあたりか

組み込みLinuxの競合になりそうなのは、Android, iOSあたりになるんだろう。

実際Android搭載のハードウエアは増えているようだし、iOS CarPlay もiPhone以外のハードへのアプローチの1つとも言える。

オープン性で言えば Web > Android > > > iOS で、Webには有利。

Webのオープンさ、実績の豊富さ、スマホファーストな時代でも廃れないメインツールとしての有利なポジショニングがかなり強みになると思う。

UIをWebにするのは新しいアプローチだけど、WebがハードウエアのUIのメインになる可能性は高いと思う。

# Web系エンジニアには新しいフィールドの誕生

実はIoT・ハードウエアは、これからWeb系エンジニアの新しい活躍のフィールドになってくるのでは？

UIがWeb化すると、フロントエンジニアだったり、フロントと通信するAPIをつくるWebアプリエンジニアも活躍の場が広がる。

Linuxそのものの設定になれば、インフラエンジニアも登場する。

ハードウエアを扱うメーカーのエンジニアは、今までのスキルセットで行くと、Webとは縁遠いと思われる。
なので、今いるWeb系エンジニアへの需要がハードウエア業界から来る。ということが、これから起きそう。

ハードウエアは縁遠い、独特の世界と思っていたけど、向こうから近づいてきてくれた印象。

意外にもハードウエア業界は、Web系エンジニアの新しい活躍の場になってくれるのでは？
