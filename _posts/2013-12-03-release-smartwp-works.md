---
title: 新サービス スマートWP Works リリースしました
date: Tue, 03 Dec 2013 12:30:49 +0000
author: Hiro Fukami
layout: post
permalink: /2013/12/03/release-smartwp-works/
categories:
  - Business
tags:
  - notices
  - PaaS
  - release
  - service
  - SmartWP
  - wordpress
  - お知らせ
---
本日、私の会社 株式会社シェイクソウルで、「スマートWP Works」というサービスをリリースしました。  
[<img class="alignnone size-full wp-image-1417" alt="SmartWP-Works_slim" src="/images/2013/12/SmartWP-Works_slim.png?resize=600%2C300" data-recalc-dims="1" />][1]  
このサービスはリーンスタートアップの手法を採用して、20名ちょっとにインタビューしサービスの修正を繰り返して生み出したサービスです。

## スマートWP Worksについて

サービス内容は、WP制作者のための制作サイトで見た目のデザインを作る時や顧客のレビューをする時に使うWordPressサイトです。  
これを使うことでテキストエディターさえあればWPサイトの制作がすぐに始められるだけでなく、制作作業も効率的に大きく変えてくれるようになります。

まるでローカルPCがサーバに直接つながっているかのように思え、今までより早くサイトを制作できるようになります。

通常ローカルPCに開発環境(Web + php + MySQL)をセットアップして行う場合が多いですが、WP制作者のコアスキルセットはCSS, HMLTコーディングなのでセットアップも一苦労だったり、バージョン管理せずに古いまま使い続けてしまうことがあります。  
メンテされないローカル環境はいざサーバにファイルをアップロードした時に不整合を生じさせる可能性があるので、理想的には &#8220;制作環境 = サーバ環境&#8221; となります。しかし、ローカルでファイル編集をした後にアップロードする手間があり、見た目のチェックは都度ブラウザで確認するので、その作業をサイトを納品するまでに100回以上行うかもしれません。100回分のアップロードは積み上げると結構な時間になります。1回のアップロードに 40秒かかるとして 40秒 x 100回 = 4000秒 = 1.1時間 になります。

Woks ではこのアップロードの手間をまるごと省くことができます。これを実現するためにDropboxを用いています。

## Dropboxでいろんなことができるようになった

[<img class="alignnone size-full wp-image-1322" alt="dropbox-logos_dropbox-logotype-blue" src="/images/2013/10/dropbox-logos_dropbox-logotype-blue.png?resize=446%2C171" data-recalc-dims="1" />][2]  
サーバへのファイルアップロードに Dropbox を使うのはとてもユニークだと思いますが、Dropboxの機能も含め色々便利な機能を提供できることになりました。

1.  ファイルアップロードの手間がなくなる
2.  ローカルPCへのセットアップがなくなる
3.  リモートからレビューと進捗チェックができるようになる
4.  共同作業ができるようになる
5.  ファイルバージョン管理ができるようになる

詳しくは <a href="http://www.shakesoul.net/smartwp-works" target="_blank">スマートWP Works のサービスページ</a> で書きましたので、参照いただければです。

## そしてこれから

スマートWPはスケーラブルな本番環境と今回リリースした Works というラインナップで、WordPressのためのクラウドサービスを提供していきます。まだリリースしたばかりなので収束することなくより良いサービスへ修正していければと思っています。

それと WordPress という今や世界中で拡大し続けるこのセグメントでもう一つ異なる側面のサービスを出せたらと思っています。こちらもモックができたら想定ターゲットに見せて行きたいしこのブログでも紹介できたらと思います。

お楽しみに！

 [1]: http://www.shakesoul.net/smartwp-works
 [2]: http://www.shakesoul.net/smartwp/dropbox-upload