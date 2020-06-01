---
layout: post
title: "ようやく4Kモニター購入 2020年版"
date: 2020-06-01 12:00:00 +0900
comments: true
categories:
  - Shopping
tags:
  - 4K
  - monitor
  - macbook
  - dell
description: 4Kモニターもようやく揃ってきた感があり、23.8インチよりも多く情報を表示したくなり再び4Kモニター購入。落ち着くまでちょっと色々あったので、そのお話。
image_out: https://ws-fe.amazon-adsystem.com/widgets/q?_encoding=UTF8&ASIN=B07GYX7G5Z&Format=_SL250_&ID=AsinImage&MarketPlace=JP&ServiceVersion=20070822&WS=1&tag=dsea-22
---
<a href="https://www.amazon.co.jp/gp/product/B07GYX7G5Z/ref=as_li_ss_il?ie=UTF8&psc=1&linkCode=li3&tag=dsea-22&linkId=b9e45b5b03b862711bce09997f16f99d" target="_blank"><img border="0" src="https://ws-fe.amazon-adsystem.com/widgets/q?_encoding=UTF8&ASIN=B07GYX7G5Z&Format=_SL250_&ID=AsinImage&MarketPlace=JP&ServiceVersion=20070822&WS=1&tag=dsea-22" ></a><img src="https://ir-jp.amazon-adsystem.com/e/ir?t=dsea-22&l=li3&o=9&a=B07GYX7G5Z" width="1" height="1" border="0" alt="" style="border:none !important; margin:0px !important;" />

[Dell 4Kモニター 31.5インチ U3219Q](https://amzn.to/2Me9G8N)

2015年に初めて[4Kディスプレイを買ってみて失敗した話を書いたけど](/2015/11/26/dell-4k-display/)、2020年に入ってようやく市場にも4Kモニターが出回るようになり、作業的にも 23.8インチ 2560x1440 よりもっと情報表示したい(特に Xcode 辛い。。)なと思い、再び4Kモニターの購入検討をすることにした。

<!-- more -->

# Amazonレビューとヨドバシ店頭チェック

まずはどんな機種が出ているのかAmazonで確認。モニターサイズは現状の23.8インチより大きくて、4K(3840x2160)表示可能なもの。
サイズは31.5インチか。
レビュー4以上、レビュー分布の低評価の分布が少ないもの、レビュー内容がフェイクっぽくないもので絞る。

前回失敗した時のように MacBook をつないだ時に文字が読めるかどうか、スケール設定しても満足な情報表示スペースがあるかどうかも大事なポイント。こればっかりは実際に繋いでみないとわからない。

ということで、店頭表示している店舗に行ってみる。新宿西口のビックカメラは意外にも31.5インチ 4Kモニターがほとんど展示していなかった。その足でヨドバシに行く。こちらはいくつも展示してあった。

その中で目についたのは USB Type-C のポートが1つ付いているLGのモニター。
予算的には 32UL750-W かな。

現時点で Amazon's Choice になっている。

<a href="https://www.amazon.co.jp/LG-32UL750-W-DisplayHDR600-Type-C%E3%80%81DP%E3%80%81HDMI%C3%972-FreeSync/dp/B07MXDZFVV/ref=as_li_ss_il?__mk_ja_JP=%E3%82%AB%E3%82%BF%E3%82%AB%E3%83%8A&dchild=1&keywords=32UL750-W&qid=1590976945&sr=8-5&linkCode=li3&tag=dsea-22&linkId=88c58b3c561822952710591f42b8d759" target="_blank"><img border="0" src="https://ws-fe.amazon-adsystem.com/widgets/q?_encoding=UTF8&ASIN=B07MXDZFVV&Format=_SL250_&ID=AsinImage&MarketPlace=JP&ServiceVersion=20070822&WS=1&tag=dsea-22" ></a><img src="https://ir-jp.amazon-adsystem.com/e/ir?t=dsea-22&l=li3&o=9&a=B07MXDZFVV" width="1" height="1" border="0" alt="" style="border:none !important; margin:0px !important;" />

[LG モニター ディスプレイ 32UL750-W](https://amzn.to/3cfcCMU)

店員さんに Macbook つないでみてよいか聞いてみて快諾いただいた。ケーブルも貸していただいてさすがヨドバシ、分かってらっしゃる。店員さんの優しさに感謝しつつつなぐ。

うん、表示の感じも良さそう。スケール設定も変えてみて最も細かい時は文字が小さすぎるようだが、1つ大きくすれば良さそう。

結局、LG モニター 32UL750-W を購入。

# ブラックアウト発生

購入した LG モニター 32UL750-W に Macbook をつないで使い始める。
モニターのファームウエアのせいなのか、画面表示や切り替わりに若干のタイムラグがあるけど、作業する分には問題なし。

と、普段どおりに仕事していると画面が真っ暗になった。しばらくしてもとに戻る。時間にすれば2〜3秒くらいか。使い続けて現象が分かってきた。

Macbook を USB Type-C につないで数時間たつとブラックアウトが生じる。1秒から5秒くらいして元に戻る。その際は Macbook が外部ディスプレイの接続し直しのような挙動をしないので、単純にモニター側で表示が消えてしまっているみたい。Macbook 側がスクリーンセーバーだろうが、通常のアプリを使っている状態であろうが、ブラックアウトが生じる。
別ポートの HDMI につないでいる [Amazon Fire TV Stick 4K](https://amzn.to/2zQxrAZ) では現象が出ないみたい。

設定でなんとかならないかいろいろ試したが解消せず、LGのサポートに問い合わせ。結局、カスタマーセンターに送ることになった。

# LG カスタマーセンターでは再現せず

LG カスタマーセンターから連絡をもらい、再現しない旨を伝えられる。
とりあえずの対処としてファームウエアの最新版が出ているので、それを入れて返却されることになった。

Macbook につないで作業を始めるもまたもブラックアウト発生。
多分 Windows だと生じないんだろうなぁ。細かなところはわからないけど Macbook 12インチとの相性なんだろうなぁ。

ランダムなタイミングでのブラックアウトはストレス極まりないので、これ以上の使用は諦めてメルカリで売った。もちろん Macbook 12インチにつないだ時にブラックアウト発生することは説明した上で購入者がいた。

# Dell 4Kモニター 31.5インチ U3219Q で正解

選定し直し。4Kでモニターサイズは31.5インチで良いことは分かっているので、USB Type-C でつなげるモデルをAmazonで探す。

今まで使ってきたモニターで特に不満もなかった Dell を見てみる。

U3219Q という機種が当てはまった。

<a href="https://www.amazon.co.jp/Dell-31-5%E3%82%A4%E3%83%B3%E3%83%81-%E3%83%95%E3%83%AC%E3%83%BC%E3%83%A0%E3%83%AC%E3%82%B9-%E3%83%97%E3%83%AC%E3%83%9F%E3%82%A2%E3%83%A0%E3%83%91%E3%83%8D%E3%83%AB3%E5%B9%B4%E4%BF%9D%E8%A8%BC-U3219Q/dp/B07GYX7G5Z/ref=as_li_ss_il?__mk_ja_JP=%E3%82%AB%E3%82%BF%E3%82%AB%E3%83%8A&dchild=1&keywords=dell+4k+32&qid=1590982358&sr=8-2&linkCode=li3&tag=dsea-22&linkId=dacc087282814e8a51378792e004df48" target="_blank"><img border="0" src="https://ws-fe.amazon-adsystem.com/widgets/q?_encoding=UTF8&ASIN=B07GYX7G5Z&Format=_SL250_&ID=AsinImage&MarketPlace=JP&ServiceVersion=20070822&WS=1&tag=dsea-22" ></a><img src="https://ir-jp.amazon-adsystem.com/e/ir?t=dsea-22&l=li3&o=9&a=B07GYX7G5Z" width="1" height="1" border="0" alt="" style="border:none !important; margin:0px !important;" />

[Dell 4Kモニター 31.5インチ U3219Q](https://amzn.to/2Me9G8N)


価格を調べると Amazon 表示価格が最安みたい。LG 32UL750-W より4万円ほど高く10万ちょっとの金額だけど。安定運用のためこれはしょうがないと思い、そのまま Amazon で買う。

# ようやく安定運用

2日後くらいに届いて早速仕事で使い始める。

ブラックアウト発生なし、この状態が当然だけど。画面の切り替わりもLGに比べて早いし、余計に太ったファームウエアではないみたい。単純に表示することに特化していて、ストレスなし。ポート数も十分でアダプター代わりにもなる。

ということで、色々時間がかかってしまったが、ようやく落ち着いて仕事できるようになった。

Macbook 12インチにつなぐ今オススメの4Kモニターは Dell 4Kモニター 31.5インチ U3219Q ということでした。

[Dell 4Kモニター 31.5インチ U3219Q](https://amzn.to/2Me9G8N)

<iframe style="width:120px;height:240px;" marginwidth="0" marginheight="0" scrolling="no" frameborder="0" src="https://rcm-fe.amazon-adsystem.com/e/cm?lt1=_blank&bc1=000000&IS2=1&bg1=FFFFFF&fc1=000000&lc1=0000FF&t=dsea-22&o=9&p=8&l=as4&m=amazon&f=ifr&ref=as_ss_li_til&asins=B07GYX7G5Z&linkId=a5c74db5e8693c7fac4e7f7101916b58"></iframe>

このモニターを使い始めて周辺環境を変えたこともあり、今度はそのことを紹介予定。
