---
title: デザイニングインザブラウザとアジャイル開発の根本は同じ
date: Tue, 21 Jan 2014 10:20:42 +0000
author: Hiro Fukami
layout: post
permalink: /2014/01/21/designing-in-the-browser-vs-agile-development/
categories:
  - Business
tags:
  - agile
  - design
  - Designing in the browser
  - develop
  - tech
  - thinking
  - web
---
## デザイニングインザブラウザ って？

昨年末に **デザイニングインザブラウザ** という単語に出会った。

[スマートWP Works][1] を使っていただいた森川さんからのフィードバックより

> 改めて感じたことですけど（スマートWPのサイトにも書いてありますが）、このサービスって、今後インブラウザデザインの手法が主流になってくるといっそう威力を発揮しそうですね。
> 
> CSSフレームワークなどでささっとプロトタイプを作って、クライアントなどプロジェクトメンバーとシェアして、チューニングをかけていく……という感じで。
> 
> スマートWP＋Googleハングアウト、というワークフロー面白そうです。

恥ずかしながら **インブラウザデザイン** の意味が「？」だったので調べると。  
元々は英語で **Designing in the browser** と書いて、日本語でカタカナにする時に「デザイニング・イン・ザ・ブラウザ」とか「デザイニング・イン・ブラウザ」とか、より略して「インブラウザデザイン」とか言ったりするらしい。  
<!--more-->

  
意味は、

> デザインカンプは作成せず、簡単なワイヤーフレームと配置するコンテンツ（画像や文章）を用意して、HTMLとCSSをコーディングしながら『ブラウザー上で直接デザインする』のが、レスポンシブWebデザインにおける効率的な手法です。  
> この手法を「デザイニングインブラウザ（Designing in Browser）」といいます。  
> 引用元: <http://matome.naver.jp/odai/2135434186061686301>

他の参考URL

*   <http://ascii.jp/elem/000/000/699/699812/>
*   [http://css.studiomohawk.com/in-browser-design/2011/04/16/designing\_in\_browser/][2]
*   <http://design-spice.com/2012/06/29/designing-in-the-browser/>

ということで、Webデザインにおける新しい手法ということがわかりました。

## アジャイル開発と同じだ！

自分のフィルターを通して言葉に置き換えると、

**レスポンシブルなWebサイトを制作するにはもうPhotoshopなどで作る見た目の完成イメージはいらなくって、最初からHTMLとCSSでコーディングして顧客の望む完成形に近づけていく手法。**  
と理解しました。

と、ここで思ったのがソフトウエア開発におけるアジャイル開発の思想と全く同じであること。  
アジャイル開発に当てはめて上記の文章を言い換えると、

**変化の早い時代においてはもうウォーターフォール型の設計書に基づいたソフトウエア開発は失敗を招くだけで、素早くモックを作りデモとフィードバックから修正を繰り返して顧客の望む完成形に近づけていく手法。**

デザインの世界では「デザイニングインザブラウザ」、ソフトウエア開発では「アジャイル開発」と単語は違えど、目的は同じ。

ちなみにアジャイル開発を理解する時に読んだ本は、  
[<img border="0" src="http://ws-fe.amazon-adsystem.com/widgets/q?_encoding=UTF8&#038;ASIN=4274066940&#038;Format=_SL160_&#038;ID=AsinImage&#038;MarketPlace=JP&#038;ServiceVersion=20070822&#038;WS=1&#038;tag=dsea-22" />][3]<img src="http://ir-jp.amazon-adsystem.com/e/ir?t=dsea-22&#038;l=as2&#038;o=9&#038;a=4274066940" width="1" height="1" border="0" alt="" style="border:none !important; margin:0px !important;" />  
[アジャイルプラクティス 達人プログラマに学ぶ現場開発者の習慣][4]<img src="http://ir-jp.amazon-adsystem.com/e/ir?t=dsea-22&#038;l=as2&#038;o=9&#038;a=4274066940" width="1" height="1" border="0" alt="" style="border:none !important; margin:0px !important;" />

[この本のレビューを書いたブログポスト >][5]

で、実際のプロジェクトで色々試すことができました。スケジュールを大きく壁に貼ってその前で朝会やったり、週1の定例ミーティングで顧客とサイクル作ったりとか。大変有意義な経験でした。

こういった手法に行き着いた時代背景とか条件を考えてみると、

*   時間とコストを掛けて顧客の望まないものを作ってしまうという数多くの失敗を重ねてきた
*   最後に出来たものを見せる手法は途中顧客が確認できず、リスクが高すぎて失敗しやすい
*   最初に示したものがそのまま最終形になることはまずない
*   何が正しいか最初の段階では分かりづらくなった
*   顧客は時代の変化を感じたりして気まぐれに欲しいものが変わる

という感じかと思う。デザインの世界でもシステム開発の世界でも同じ背景を元に同じような苦い経験を重ねてきたのではないか。

業界は違えど、そんな先人たちの失敗の経験を元に作られた新しい手法がそれぞれ「デザイニングインザブラウザ」と「アジャイル開発」なのだと思いました。

## 同じ目的に進んでいる

つまり、「デザイニングインザブラウザ」と「アジャイル開発」の目的は、

**なるべく失敗するリスクを減らし顧客の望むものを正しく収める**

ことだと思う。

手法上のポイントは、

*   なるべく動くものを早い段階で顧客に見せる
*   「開発・制作 => 顧客へデモ => フィードバックを得る => 修正」のサイクルを一定サイクルで繰り返す
*   開発・制作規模は小さいパッチでサイクルを回しやすいようにする

というところで、おそらくウォーターフォール型に慣れてしまっている人は綺麗に設計しようとしてなかなか顧客に見せようとしないが、未完成でも見せてとにかく**サイクルを回す**ことに注力するのが大事。

経験上、今までの受託で途中でデモする威力はとても大きいと思っていて、顧客も納得しながら進められて、関係を良好に維持しながら作れることはとても良いこと。

## 推進者は設計者から制作者・エンジニアへ

最初に行う設計の仕事量を減らし素早く作るとなると、従来の設計者の仕事量は減るしそれだけ重要ではなくなる。  
素早く作れることが重要で、**作っては修正**をできる人がプロジェクトの推進者になる。

今まではシステム開発でも親受け会社が設計だけやって下請けに開発させて、設計がいい加減だから下請けがデスマーチ。みたいな感じだった。  
実際自分は新卒で入った会社で設計しかしないSEだったのだけど、感覚的に「設計する人＝偉い」みたいな関係はおかしいなと思っていたけど、ようやくこのパワーバランスが壊れる時代が来たのではと思う。

そうなると「親受け会社＝偉い」も崩れて、顧客に貢献できるのは下請けだった制作者・エンジニアになる。  
単純に作れればいいということではなく、顧客とやりとりするには折衝能力、時間コスト管理能力とかも必要だけど、プロジェクトを推進するための必要条件と確実に言える。

## 面白い時代に入った

パワーバランスが崩れたり、今まで重要視されていたものがそうではなくなったりするのは、世の中がより良い方向に進んでいる証拠だと思っていて、その様子は世界が変わる感じがしてとても面白い。

会社のネームバリューがモノを言う時代は終わり、小さなチームや個人が世の中に与える影響力がますます大きくなっていくのだろう。  
そうなってくると、単純に良いもの良いパフォーマンスをした人たちが評価されやすくなる。評価は対価につながって会社規模に関係なく、良いものを提供できる人がそれに応じた対価を受けられるようになる。

面白い時代に入ったと思う。

そういう世の中に近づけるように新しいアイデアをねっていたりするのだけど、もう少し固めて今後このブログで紹介したいと思っていますので、乞うご期待。

 [1]: http://www.shakesoul.net/smartwp-works
 [2]: http://css.studiomohawk.com/in-browser-design/2011/04/16/designing_in_browser/
 [3]: http://www.amazon.co.jp/gp/product/4274066940/ref=as_li_ss_il?ie=UTF8&#038;camp=247&#038;creative=7399&#038;creativeASIN=4274066940&#038;linkCode=as2&#038;tag=dsea-22
 [4]: http://www.amazon.co.jp/gp/product/4274066940/ref=as_li_ss_tl?ie=UTF8&#038;camp=247&#038;creative=7399&#038;creativeASIN=4274066940&#038;linkCode=as2&#038;tag=dsea-22
 [5]: http://hirofukami.com/2009/01/19/post/