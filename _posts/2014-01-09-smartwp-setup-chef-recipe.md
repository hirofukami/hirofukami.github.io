---
title: スマートWP構築のChefレシピを公開します
date: Thu, 09 Jan 2014 11:52:32 +0000
author: Hiro Fukami
layout: post
permalink: /2014/01/09/smartwp-setup-chef-recipe/
categories:
  - Tech
tags:
  - chef
  - public
  - recipe
  - Server
  - setup
  - SmartWP
  - tech
---
去年夏に Chef に出会い、[スマートWP][1]のセットアップ方法を変えました。  
<img src="http://docs.opscode.com/chef/_static/opscode_chef_html_logo.png?w=830" alt="" data-recalc-dims="1" />  
今までは「AWS EC2 を手動でセットアップして AMI 保存」を繰り返すことでサーバをバージョンアップしてきた方法から、まっさらな AWS AMI を Chef で一気にセットアップするように切り替えました。

何が良いかというと、

*   最新のバージョンが常に保たれる 
    *   AWS AMI もバージョンアップされるし、各アプリケーションも常にバージョンアップされるので
    *   AMIを更新し続けると古いバージョンのままになってどこかでバージョンアップの作業が必要になる
*   すべての作業内容がレシピに残る 
    *   どうしても手動だとやったことの記録漏れが出がち
*   手動よりも圧倒的に早い

というところかと思う。

そんな Cehf で作った[スマートWP][1]のレシピを公開します。

このレシピで作られたスマートWPのサーバ環境は私の個人ブログ [hirofukami.com][2] と会社のサイト [ShakeSoul inc.][3] があって、負荷状況に応じて自動的にサーバを増減させるオートスケーリングが動くようになっています。

ご興味のある方は [スマートWPのサービス紹介ページ][1]を見てみてください。

<center>
  <br /> <a href="http://www.shakesoul.net/smartwp"><img src="http://www.shakesoul.net/wp-content/uploads/2013/05/SmartWP-Scale-h300.png?resize=327%2C180" class="alignnone" data-recalc-dims="1" /></a><br />
</center>

<!--more-->

レシピは2つ使っていて、他の環境とも共用しているベースとなるセットアップの後にスケーラブルな構成のためのレシピを適用しています。

では、以下。私の [Github Gist][4] からの貼付けです。

まずはベースの設定。recipe[swp_setup]です。 

{% gist 8328203 swp_setup_default.rb %}


続いて、recipe[swap_scale-org]です。

{% gist 8328145 swp_scale-org_default.rb %}

これを nodes/[hostname].json で、

{% gist 8328241 node.json %}

のように書いて適用させています。

こんな感じで、Webサーバとしては nginx + php-fpm、それに Dropbox linux と s3fs を使っているのがお分かりかと思います。 今後、ここで読み込んでいるテンプレートの nginx のコンフィグとかなんかも公開しようかなと思っています。

色々やっている方とディスカッションしてみたいので、違うやり方しているよとかこっちのほうが良いよみたいなのがあればフィードバックください〜

あと、Chef 学びたいとかスケーラブルなサーバ環境つくりたいとかもあれば相談いただければです。

 [1]: http://www.shakesoul.net/smartwp
 [2]: http://hirofukami.com
 [3]: http://www.shakesoul.net
 [4]: https://gist.github.com/d-sea