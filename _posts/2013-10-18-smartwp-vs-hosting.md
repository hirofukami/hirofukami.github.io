---
title: スマートWordPressが他ホスティングと違うところ
date: Fri, 18 Oct 2013 11:03:48 +0000
author: Hiro Fukami
layout: post
permalink: /2013/10/18/smartwp-vs-hosting/
categories:
  - Business
tags:
  - Business Model
  - hosting
  - PaaS
  - Server
  - SmartWP
  - wordpress
---
<a href="http://www.shakesoul.net/smartwordpress" target="_blank">スマートWordPress</a> をリリースしようと思った理由は、現状ある他社のホスティングサービスにはない視点と課題解決へのアプローチが取れるからだと思っています。  
その異なる視点をどこに置いているかを紹介したいと思います。

今まで WordPress制作者やブロガーにインタビューしてきましたが、現状抱える解決したいことは大体3つの課題に集約されました。

1.  「WordPress制作や運営を楽に行いたい」
2.  「障害に悩まされずに安心したサイト運営がしたい」
3.  「サーバスペックや容量を気にしたくない」

1つずつ見ていきましょう。

> 「WordPress制作や運営を楽に行いたい」 

ユーザがWordPressに対して行うことは記事を書いたり、テーマカスタマイズであって、それ以外の部分はしたくない。楽にできない部分が現状あるということ。

> 「障害に悩まされずに安心したサイト運営がしたい」

WordPressはサーバ上で動くのでサーバに依存することになる。24&#215;365動くサーバに障害が起きないことはないので、サーバ障害の心配は常に存在する。そして、サーバ障害が起きても自分たちが解決できないというジレンマを持っています。

> 「サーバスペックや容量を気にしたくない」

現在のホスティングサービスは大体長ったらしいスペック表がある。CPU、メモリ、ディスク容量あたりが一般的。しかし、ユーザから見ると実際CPUやメモリがどの程度必要かはわからないのでスペック表からは判断できない。また、時代はクラウドと言われているのになぜスペックや容量の制限があるのか、クラウドの特徴を得られていない違和感を感じている。

これらの意見がユーザから出てしまう原因は、サービス提供者とユーザの間に大きなギャップがあるから、うまくサービス提供者がユーザの求めるものをサービスに反映出来ていないとも言えます。  
これらの課題があるからこそスマートWordPressが解決すべき価値がそこにあり、参入するチャンスがあると思っています。  
大きなギャップは何か。レイヤで整理すると分かりやすいと思うので図を示します。

[<img class="alignnone size-full wp-image-1347" alt="20131018WP-SV.001" src="/images/2013/10/20131018WP-SV.001.png?resize=830%2C623" data-recalc-dims="1" />][1]

WordPressに関わるユーザは表の上部の黄色枠が自分たちがやろう・やりたい部分だと思っています。  
WordPress が動くことが前提で、その上でプラグインやテーマを入れてサイトを運営する。  
<!--more-->

  
そこで出た課題を見ると、サーバ障害やCPUスペックは WordPress レイヤより下なのでユーザは判断したくない、ユーザに求めてはいけない部分だと分かります。  
自分が入れたプラグインの不具合でサイトが真っ白になった。という事象はユーザがなおそうと頑張りますが、サーバ自体が落ちてしまったら自分で立ち上げ直すのではなく、WordPress サイトが表示されるまで復旧させてほしいと思うこともレイヤと照らし合わせると理解できます。

今現在ユーザの求めるレイヤまで提供できているサービスは共有レンタルサーバになります。サーバ運用はおまかせだし、ワンクリックインストールでサーバの知識がなくてもWordPressサイトが簡単に用意できます。  
<a title="ロリポップ WordPress 大規模改ざんの原因予想と思ふところ" href="http://hirofukami.com/2013/09/02/lolipop-wordpress-trouble/" target="_blank">DBの改ざん事件が起きて</a>セキュリティの懸念があるのに、また同居しているサイトによって安定したパフォーマンスが出ないというデメリットがあるにも関わらず、非常に多くの WordPress サイトに使われています。  
共有レンタルサーバは10年以上前からあるサービスですが、ここ5年で登場した IaaS、VPS に移行していかない理由はレイヤが足りないからだと認識しています。

ギャップが生じやすい理由としてホスティング会社の歴史もあります。  
ホスティング会社はハウジングの派生でサービスが始まっているので、サービスを作る際に低レイヤから固めて、選択の幅が出てしまう部分はユーザに選ばせるスタンスを取ってきました。これはハウジングの経験からカスタマイズ性の高いほうが当時の顧客にはマッチしやすかったからです。カスタマイズ性が高いということはマスではなくエンタープライズが強いことになります。  
WordPress 制作者やブロガーは当時の顧客にはマッチしない新しいカテゴリーなので、カスタマイズ性を求めていませんし、「WordPressが動く環境」というシンプルな要求です。

では、選択の幅が出てしまうサーバスペックはどうすればいいかとなると、そこがクラウドの特徴を活かす・活かせる部分になります。  
AWS はサービス名に Elastic という単語と好んで使いますが、「サーバのスペックを固定化しない = スペックが動的に伸縮自在である」機能を用意して対応します。具体的にはソリューションの紹介の時に書きたいと思います。

スマートWordPressはユーザが求める高いレイヤまでをサービスとして提供します。機能というよりユーザビリティ、ユーザ体験そのものを今までとは違った次元で提供できるはずです。

近々、クローズドβリリース予定ですので、リリース時に利用したい方は <a href="http://www.shakesoul.net/smartwordpress" target="_blank">スマートWordPressサービスページ</a> からメール登録いただければと思います。

クラウドサービスのカテゴリで言うと PaaS に属しますが、ある用途に特化した場合 IaaS より PaaS の方がユーザの課題を解決することになり、よりビジネスとして成り立ちやすいと思っています。今回は WordPress というセグメントですが、これは別のセグメントでも成り立つことだと思っています。

次回は課題に対してどういうソリューションを提示するのか具体的にご紹介していきます。

 [1]: /images/2013/10/20131018WP-SV.001.png