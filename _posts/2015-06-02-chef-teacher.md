---
layout: post
title: "Chef講師をしたのでまとめ"
date: 2015-06-02 10:00:00 +0900
comments: true
categories:
  - Business
---
<img src="/images/2015/06/20150602-teacher.png" width="640">

Chef の研修講師をしました。

具体的には、近年のインフラ業界での様子解説とDevOpsの概要、chef の knife solo の演習です。

![](/images/2015/06/20150602-chef-logo.png)

今年1月からとあるエンタープライズSIerさんの企業内研修の1プログラムとして実施してきました。

全6回で、

* 60分 x 2 座学
* 90分 x 3 演習
* 60分 x 1 成果発表とクロージング

という構成。参加者数は10名程度。

5月末で一区切りついたので、どんなのことを意識してやってきたのか。また、やった結果どんな成果が産まれたのかまとめてみたいと思います。

<!-- more -->

# 気にしたこと

## 全般的に

単にツールを使うことを覚えるだけではなくて、大局的に捉えてDevOpsやインフラ革命の流れの中で産まれたツールであることを位置づけたかった。

このツールを使いこなせることで大きなメリット得られるので、個人としての武器にして欲しい。個人がしっかり使えるようになること。

## 演習

### 動かす楽しさ

動いた！という快感を味わって演習のモチベーションにしてもらう。作業の自動化とはどういうことなのかを実際味わうことが重要だと思ったので、解説する前にまずはサンプル Cookbook を手元の環境で動かしてもらった。

この狙いとしては、動いたものを見ると動かすためにどんなことが必要か。という疑問が出るので、それを埋めていく感じに進められる。学ぶが先に来ると単なる座学になってしまう。実際とリンクしにくい。順番を入れ替えることで学ぶ意欲を掻き立てる。

### シンプルかつ有用なサンプル

いくつかの重要な要素が詰まっていつつもシンプルなサンプル Cookbook を作成した。

具体的には、nginx をインストール、confファイルを置き、サンプルindex.htmlを置く。index.htmlファイルは templete にして attribute を設けて自由に変更できるように。

実行結果が視覚的にわかりやすく、ブラウザでアクセスして確認できる。


# やりたかったこと

## ちゃんと動くことへのコミットする

演習最後に成果発表の場を設けた。各人が作った Cookbook を動かす様子をその場で見せるようにした。確実に意図したとおりに動くCookbookを作ることを求めた。

小さくてもいいから各人の成果を確実に残すことが重要。

## 自由に作れる

自由に作れる環境。
成果は自由に自分なりのCookbookを作ってもらうことにした。

単にサンプルをいじるだけでは、いざ作ろうと思ってもできない。何もない状態から作れる力が必要なので自由に発想してもらった。


# 気づいたこと

## 演習環境

演習環境として用意されたのは、検閲ツールの入ったPC、コンテンツフィルタリングのかかったインターネット環境だった。予想はしていたが、実際の演習の作業効率に支障が出た。これらの制約がなければ倍以上のスピードで進められたと思う。

chef はオープンなツールなので使用する環境もオープンでなければならない。

## Linux への知識

chef は人の手で行う作業を肩代わりしてくれるので、そもそも Linux 上で何ができるか理解していないと使えない。通常人間が手動で作業する場合どうすればよいかがイメージして、作業を順序立てられないと recipe が作れない。

受講者個人の Linux の精通度合いが作った Cookbook の充実度につながった部分があった。

## 職場での課題

最終回の研修後に懇親会を開いていただいたのだけど、そこで受講者と話してわかったことがあった。

個人としては chef を是非仕事で使いたいが、それを職場で使っていけるかが結構大きな課題になっていることがわかった。エクセルの手順書が多いようだが、それを recipe ベースにどう変えるか。拒否反応を示されるのではないかと心配していた。

職場の具体的様子を知らないので解決策は提示できなかったが、企業内でそれを推奨する文化がまず必要なのではないかと思う。
トップダウンというかスタッフの作業環境に責任を持つ立場の人が、ツールがもたらすメリットを理解する。それを導入することでどう良くなるのかわかっていないと、結局個人が頑張っていますねだけの話に終わってそのメリットがチームに伝搬することがなくなる。


# 伝えたこと

## bash はなるべく使わない

成果発表の recipe を見ると、bash を多用していた。

手動での作業手順をそのまま bash にすればわかりやすいので頼りたくなるのは理解できる。しかし、chef からは bash の実行状況が見えない。条件に引っ掛けることもできない。だからあまり使わず、用意されている reources をなるべく使うほうが良いよ。と話した。

## オペレーターではなく使いこなす人になろう

chef は最も優秀なオペレーター。人間の手動作業よりも100倍以上早く確実に作業をする。そのオペレーターと自分の手動作業で張り合うのはナンセンス。
張り合うのか使いこなせるようになるのかは大きな違い。使いこなす立場になって欲しい。

大きな効果をもたらすツールなので、それを使いこなして個人の武器にして欲しい。


# 結果

成果発表は想像以上に全員しっかりまとめてきた。動くものを発表できていた。各人それぞれ異なった内容になっていた。

今の仕事と照らして recipe を作ってみた人がいた。すでに導入しようと動き始めていた。

また、自分の家でサーバたてて knife solo を動かしてみたというエピソードが何名から聞けた。

こういった自主的なアクションが生み出されていた。一方的に押し付けられたものではない証拠。

この研修を通じてエピソードがいくつも生み出されていたのは非常に嬉しかった。

**「今まで受けてきた研修の中で一番作る楽しさを感じることができた。」** という声もあった。

個人的にはこれが最大の褒め言葉だと思っている。


# 講師お仕事絶賛募集中

こんな感じで行っております。

2015年後半も引き続き Dev と Ops 間を埋めるような講師事業をやっていきます。ただ今募集中です。

参考に [会社のWebにも講師事業ページ](http://www.shakesoul.net/tech-lecture) も見ていただいて、ご興味あれば [@d_sea](https://twitter.com/d_sea) や [会社のコンタクトフォーム](http://www.shakesoul.net/#contact) から連絡いただければです。
