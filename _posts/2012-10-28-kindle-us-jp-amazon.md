---
title: Kindle利用時にUS/JP Amazonアカウントでちょっと混乱したのでまとめ
date: Sun, 28 Oct 2012 02:37:00 +0000
author: Hiro Fukami
layout: post
permalink: /2012/10/28/kindle-us-jp-amazon/
tumblr_hirofukami_permalink:
  - http://hirofukami.tumblr.com/post/34459176206/kindle-us-jp-amazon
  - http://hirofukami.tumblr.com/post/34459176206/kindle-us-jp-amazon
  - http://hirofukami.tumblr.com/post/34459176206/kindle-us-jp-amazon
tumblr_hirofukami_id:
  - 34459176206
  - 34459176206
  - 34459176206
original_post_id:
  - 64
  - 64
  - 64
categories:
  - Internet
tags:
  - amazon
  - kindle
  - tips
  - trouble
---
まだ iPad mini や Kindle 端末は買ってないのですが、iPhone に Kindle アプリ入れていたので、試しに<a href="http://www.amazon.co.jp/gp/product/B009LHC7A4/ref=as_li_ss_tl?ie=UTF8&camp=247&creative=7399&creativeASIN=B009LHC7A4&linkCode=as2&tag=dsea-22" target="_blank">ジョジョの奇妙な冒険 第1巻</a>の無料サンプルを見てみるかと思ったら、ちょっとはまったのでメモっておく。  
解決の際に参考にしたのは <a href="http://www.itmedia.co.jp/enterprise/articles/1210/27/news010.html" target="_blank">このリンク</a>。

### 元々の状況

*   Amazon.com(US) にアカウントを持っていた
*   Amazon.co.jp(JP) にもアカウントを持っていた
*   2つのアカウントは別物だけど、メールアドレス/パスワードは同じに設定していた

### 起きた現象

*   US の Apple ID で Kindle アプリをインストールしていたので、削除
*   日本の App Store で Kindle アプリをインストール
*   Kindle アプリで [Settings] &#8211; [Registration] に自分の名前が表示されているのを確認
*   Amazon.co.jp にて、[アカウントサービス] &#8211; [My Kindle] &#8211; [端末の登録] を見てみる。何も登録されていない
*   Amazon.com にて、[Your Account] &#8211; [Manage Your Kindle] &#8211; [Manage Your Devices] を見てみる。こちらに iPhone が登録されていた。

### 解消するためにやったこと

*   Amazon.com の [Manage Your Devices] にて、登録されているデバイスを Deregister して削除
*   Amazon.com のアカウントのメールアドレスを変更した  
    <img src="http://media.tumblr.com/tumblr_mckz9qxyKn1qzhrk3.png?w=830" alt="us" data-recalc-dims="1" />
*   iPhone の Kindle アプリを起動、再ログインしたけど現象変わらず
*   iPhone の Kindle アプリを一旦削除、再インストール
*   Kindle アプリを起動して、ログイン、端末の登録に漢字の自分の氏名を確認
*   Amazon.co.jp にて、[端末の登録] を確認して iPhone が表示されていることを確認  
    <img src="http://media.tumblr.com/tumblr_mckz9zcMMO1qzhrk3.png?w=830" alt="jp" data-recalc-dims="1" />

結局 US-JPで同じアカウントパスワードにしてしまったのがいけなかったのだけど、同じであることに気づくのに時間がかかってしまった。  
すでにUSとJPにアカウントを持っている人向けに Amazon は<a href="http://www.amazon.co.jp/gp/help/customer/display.html?ie=UTF8&nodeId=201049300" target="_blank">統合という機能を紹介している</a>が、これはどちらかのアカウントに依存させられてしまうので、JPを購入アカウントに選択したらUSのKindle Store で購入ができなくなってしまうデメリットがある。

だから、両方のアカウントを今後も使い分けたい場合はそれぞれ独立したアカウントにしたほうがよい。デメリットは、必要に応じて Kindle アプリのログインアカウントを切り替える手間がかかることで、自分の場合は日本の Kindle Store での購入がほとんどなので、アカウント切り替えはほとんど起きないだろうなということで、この解消方法でOKになった。