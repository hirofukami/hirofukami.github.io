---
layout: post
title: "iBeaconについて情報収集"
date: 2014-12-04 22:00:00 +0900
comments: true
categories: Business
---
![iBeacon](/images/20141204-ibeacon.png)

iBeacon が盛り上がってきたたけど、どうやってこれをビジネスに使うかな？というのが疑問で、単なる流行りだけ飛びつくと痛い目にあうので、自分なりに調べてみた。

どんなビジネスに活用できそうかの自分の考えについては後日として、とりあえず集めた情報をまとめるためのポストします。

# Beacon デバイスを作っているところ

なにはなくとも必要になる Beacon デバイス。基本的には Bluetooth がおしゃべりできればいいのだけど、常時設置できるハードウエアとして作っているところはそれほど多くなさそう。

## estimote

http://estimote.com/

Estimate, Inc. の住所はアメリカ ニューヨーク、西アメリカとヨーロッパ ポーランドにもオフィスを持つ。

提供デバイス

### Beacons

![Beacons](/images/20141204-beacons.jpg)

* iBeacon準拠
* 角ばったマウスくらいのデバイス
* 3個で$99
* アンテナが届く最大距離 70m
* iOS, Android SDK と RESTful APIがある
* SDKは3万以上の開発者に使われているらしい

<!-- more -->

### Sticker Beacons

![Sticker Beacons](/images/20141204-sticker-beacons.jpg)

* 最近予約を始めた Sticker Beacons
* 秋には出荷予定(すでに冬だけど..)
* 10個で$99
* Beacons とやりとりするためだけの小型のデバイス
* おそらく iBeacon には準拠していないのかも

### 提供アプリ

デバイスを設定するためのアプリを提供している

* iOS : https://itunes.apple.com/WebObjects/MZStore.woa/wa/viewSoftware?id=686915066&mt=8
* Android : https://play.google.com/store/apps/details?id=com.estimote.apps.main

多分、iPhone/iPad自身も iBeacon デバイスになれるはずなので、iPhoneにこのアプリを入れてデバイスにしてiPad上のアプリで確認するみたいなことができるはず。

API Documentation : http://estimote.com/api/

## アプリックス 国内メーカー

http://www.aplix.co.jp/?page_id=8620

iBeacon 端末を作ってくれるメーカー。

* estimote ほどデザインに力入れておらず、基盤むき出しのものから箱型、USBタイプまでラインナップされている http://www.aplix.co.jp/?page_id=7593
* サーバを使った暗号化で脆弱性(IDの書き換えが容易でduplicateする可能性がある)に対応する
* 渋谷パルコで tab(後述) 経由で BM1 (￥300/個)が導入された、Androidにも対応している Beaconモジュール
* 国内での実績多数 http://www.aplix.co.jp/?page_id=8620

# beacon ソリューションしている会社

## inMarket

http://www.inmarket.com/

* 主な事業はモバイルアプリを使ったマーケティングプラットフォーム提供会社
* 顧客は有名なブランド リーバイス、ペプシ、コカコーラ、P&G、ネスレなど
* みずからのブランドではなく顧客ブランドでのApp提供をしているみたい

会社紹介に、

> inMarket runs the world’s largest mobile shopper marketing platform, and has built the world’s largest network of beacon-enhanced shopping apps.

 と書いてあるので、一応iBeaconを使ったショッピングアプリとかを提供しているみたい。

beacon solution として説明されているページ : http://www.inmarket.com/retail.html

# 国内事情

## メディア

### Gigazine

iBeacon 出た 2013年に早めに追いかけていた。

* http://gigazine.net/news/20140201-ibeacon-mobile-to-mortar/
* http://gigazine.net/news/20130911-ibeacon/

### iBeacon 特集

だいたい2014年前半に特集している。

* アスキー https://weekly.ascii.jp/elem/000/000/211/211602/
* 技術サイト ギャップロ http://www.gaprot.jp/pickup/ibeacon/
* ビジネスサイト http://businessnetwork.jp/Detail/tabid/65/artid/3276/Default.aspx


## 国内での iBeacon 利用ニュース

HOME’S アプリで iBeacon を使った来店検知システム試験提供 : http://www.rbbtoday.com/article/2014/04/08/118639.html

DNPとJAL、空港内の搭乗便情報提供サービスにiBeaconを活用した実証実験 : http://news.mynavi.jp/news/2014/10/14/160/

JR東日本、iOSアプリ「東京駅構内ナビ」を公開試験。160個のビーコンでピンポイント案内 : http://japanese.engadget.com/2014/12/02/jr-ios-160//?a_dgi=aolshare_twitter

その他 : http://news.mynavi.jp/tag/0018499/

## 国内プレイヤー

### スマポ

http://www.smapo.jp/

ショップに行くとポイントが貯まる O2O サービス。

* お店をお気に入りに入れて、お店に付いたらチェックインしてポイントなどがもらえる
* 元々はチェックインする時に超音波で検出していたものに補完的に iBeacon を使って対応した
* アプリを起動しなくても iBeacon が検出できるようになった http://www.atpress.ne.jp/view/38883
* 対応店舗数が多い http://www.smapo.jp/shop/index.html?d=shop%2F
* 楽天チェックを出している。Copyright が Spotlight だから楽天からまるごと請けているのかな？ http://check.rakuten.co.jp/

### tab

http://corp.tab.do/

* 旧 頓智ドット
    * 今やCEOも変わってサービスもO2Oショッピングに変わってといる
* ユーザが気に入ったショップ、宿、グッズなどをクリップする、ユーザのリストからもクリップできる
* その場所に近づくとアプリがお知らせしてくれる
* アプリックスの BM1 を店舗に全て配っているみたい http://monoist.atmarkit.co.jp/mn/articles/1312/19/news080.html

### 日本写真印刷株式会社

特にサービス提供はしていない。

* http://smartphone-ec.net/ibeacon/
* スマートフォンECラボ : http://smartphone-ec.net/category/ibeacon

iBeacon関連ニュース発信「スマートフォンECラボ」、導入パッケージ販売、コンサルティング、有料のマーケティングセミナーやっている。


### 受託アプリ開発会社 MACASEL

http://www.macasel.jp/ibeacon/

## 電子書籍 kindle

[![iBeaconハンドブック](http://ws-fe.amazon-adsystem.com/widgets/q?_encoding=UTF8&ASIN=B00J9MHG66&Format=_SL250_&ID=AsinImage&MarketPlace=JP&ServiceVersion=20070822&WS=1&tag=dsea-22)][link]

[iBeaconハンドブック][link]

個人の方が出版された。筆者の方のブログ : http://reinforce-lab.github.io/blog/2014/03/29/ibeacon-handbook/

全般的に iBeacon 関連の本は少ない。この本を購入して読んでみたが、技術的解説が非常に詳しく iBeacon の仕様がよくわかった。ビジネス的にどう活用すべきかという視点は、ないことはないが薄め。

 [link]: http://www.amazon.co.jp/gp/product/B00J9MHG66/ref=as_li_ss_tl?ie=UTF8&camp=247&creative=7399&creativeASIN=B00J9MHG66&linkCode=as2&tag=dsea-22

## 技術情報 Qiita

* iBeaon Advent Calendar 2013 : http://qiita.com/advent-calendar/2013/ibeacon
* iBeacon tag : http://qiita.com/tags/ibeacon

## Google Adwords

"iBeacon" で広告出しているところ

1. 富士通 fujitsu.com
2. iBeacon 対応 ARアプリ開発受託会社 emprize.jp
3. iBeacon Developer Kit estimote.com iBeacon デバイス制作会社
