---
title: '今作っているサービス紹介 &#8211; WPデザイナーのための簡単自由なクラウド'
date: Fri, 19 Jul 2013 17:20:50 +0000
author: Hiro Fukami
layout: post
permalink: /2013/07/19/trying-service-smartwordpress/
geo_public:
  - 0
  - 0
categories:
  - Business
tags:
  - challenge
  - cloud
  - idea
  - service
  - thinking
---
<a title="「サービスを作って食べていけるか」のチャレンジに移行します" href="http://hirofukami.com/2013/07/12/launch-service-my-challenge/" target="_blank">「サービスを作って食べていけるか」のチャレンジに移行します</a> の反響が大きかったのでびっくりしているのですが、時代は変化したとはいえまだ脱受託でサービスだけで食べていくことはやりたくても難しい状況なのでしょう。特に私の場合は一人で外部資金にも頼らず始めているので、それもまた稀なのかもしれません。

今日は実際作っているサービスについて紹介します。  
単なるサービス内容の紹介ではなく、アイデアのきっかけや狙っている世界も含めた「思い」の部分も伝えられればいいなと思います。

<p style="text-align:left;">
  今、作って試しているサービスは「(仮)スマート WordPress」と言います。
</p>

<p style="text-align:left;">
  <a href="http://www.shakesoul.net/smartwordpress" target="_blank"><img class="size-full wp-image-1084" alt="headerlogo" src="/images/2013/07/headerlogo.png?resize=787%2C272" data-recalc-dims="1" /></a>
</p>

<p style="text-align:left;">
  WordPressデザイナーのための最も簡単に自由に使えるクラウドサービスです。<br /> WordPress.comをご存知な方にとっては「自由を得た WordPress.com」と思っていただければ良いかと思います。
</p>

このサービスを思いついた背景ですが、  
クラウドというと AWS はじめ国内のクラウド事業者が多くなってきたせいか、IaaS のイメージが強くなってきていてサーバを触ることができない人にとっては何か難しいものになってしまっている気がしていました。  
クラウドとは言いながらも、インスタンスのスペック、システム全体のキャパシティ、監視、モニタリングなどユーザが考慮しなくてはならないものが多く、その部分はいわゆるインフラエンジニアが工数かける必要があります。  
クラウドの理念は「インターネット上にある膨大なリソースを利用しアクセスすることができる」とするならば、もっと柔軟な抽象化されたサービスがあるべきだと思っています。AWS と出会った時の衝撃は、膨大なリソースを自分でコントロールできることであり、膨大な規模のサービスを自分で生み出せる可能性への興奮でもありました。

そして、私のサービスを作る上での方向性は「サービスを使うことで、出来なかったことが出来るようになる、魔法の杖になる」ということを思っていて、自転車に乗れるようになった時の気持ちよさや Mac を触ってワクワクするような感情と同じことを狙っています。

この私の考えるクラウドサービスと方向性が一致して、「ユーザが膨大なクラウドのリソースを簡単に使って価値を生み出せるサービス」というコンセプトが生まれました。  
これは fluxflex を作った時と基本同じで、世の中では PaaS と呼ばれますが思いついた時にはまだそんな単語はありませんでした。

今回、WordPress をプリインストールした環境を提供するクラウドにしています。  
なぜ WordPress かというと、WordPress ほど様々な用途の Web サイトに利用され続け、発展し続けているものは他にないからでした。周りを見ての肌感覚ですが、企業サイトやショップサイトにも利用されてそのシェアは増加する一方だと思っています。実際 <a href="http://w3techs.com/technologies/overview/content_management/all" target="_blank">CMSのシェアは58.5％にもなります</a>。  
用途ごとに気の利いたテーマ、機能を補完する充実したプラグインが、発展に大きく貢献しています。今後も WordPress の利用シーンは増える一方だと思っています。

そして、WordPress が使えるクラウド or サーバ環境を提供するサービスを調べました。WordPress.com には好きなテーマとプラグインがアップロードできずにそもそも自由度がないことや WordPress がインストールできるホスティングサービスは、元々幅広い用途を想定しているので、あまりターゲットを想定したシンプルさがないことがわかりました。特にホスティングサービスは昔からの手法で FTP を使ったファイル更新方法をとっており、これは今どきではない部分です。もっと簡単な環境が作れると思いました。

簡単な環境が提供できたとして、最も恩恵を受ける人たちは WordPress デザイナーさんだと想定しました。WordPress デザイナーは、WordPress のテーマをカスタマイズしたり必要な機能を持つプラグインを入れて、クライアントのWebサイトを作成して収めています。サーバ部分は頼まれながらも断ってしまうか or 不慣れながらも何とか動いた状態を頑張って作る。という状況かと思います。

WordPress デザイナーからすれば、基本的にはテーマとプラグインがサーバにアップロード出来れば良くて、サーバのセットアップや運用は他の誰かに任せたい。極力サーバのコマンドライン画面に触れたくないのが本音ではないでしょうか。  
なので、ブラウザから好きなテーマとプラグインをアップロードすることだけ出来るようにしました。AWS にアカウントを作る必要はなくスマート WordPress を使ってもらえるだけで良いです。あとはいつものように WordPress の管理画面で操作出来ます。

サーバのアーキテクチャとしては、AWS のオートスケーリングとRDSのマルチAZを組み合わせて、サーバがダウンしても自動的に復旧できる仕組みにしています。負荷によってWebインスタンス数をスケールさせつつ、複数台になってもインスタンス内部に生成されたファイルが他のインスタンスにも反映されるような仕組みを作って、WordPress がちゃんと動作する保証をしています。Webサーバはチューニングして高速化したものでコストパフォーマンスを最大化します。

現在、<a href="http://www.shakesoul.net/smartwordpress" target="_blank">このサービス用のWebページ</a>を作って興味ある方にメールアドレスを登録いただいています。今後、機能面のブラッシュアップや価格などを決める必要がありますが、アップデートは登録いただいた方に都度お伝えしていきます。

あわせて、WordPress をカスタマイズする案件を受託されている WordPress デザイナーさんに直接お会いして、デモを見ていただきフィードバックをもらい始めています。  
もっとたくさんの意見を得て行きたいので WordPress デザイナーの皆さん、是非連絡くださいませ！連絡方法は <a href="http://facebook.com/fukami" target="_blank">FB メッセージ</a>でもこのブログのコメントでも結構です。

良いサービスにしていくために是非ご協力をお願いします！

という感じで最後はお願いになってしまいましたが、こんなことを考えてサービスを作っているよ、ということが少しでも伝われば幸いです。