---
layout: post
title: "会社Webリニューアル"
date: 2015-03-16 10:00:00 +0900
comments: true
categories: Business
---
![ShakeSoul Web Site](/images/2015/03/20150316-visiable-shakesoul-web.png)

前回リニューアルしたのが[このポスト](http://www.shakesoul.net/2014/05/16/renewal-web.html)なので、1年弱で変えたことになります。

Webサイトのデザインは洋服のファッションと同じように流行り廃りが早くて、トレンドがどんどん変わっていきます。だから流行りの服を買うように、そのトレンドに乗り遅れている感じが出ないためにも、割りと気軽にリニューアルはしたいと思っています。

サイトはWordPressなので、テーマを入れ替えてそれにそって設定すればリニューアルは完了。保存済のブログやページに手を入れる必要がないのが、WordPressの良いところですね。

せっかく変えるなら、トレンドを盛り込むだけでなく、改善もしたい。ということで、ページ遷移のすっきり感やコンテンツ情報の重複をなくすことを改善ポイントにして作業しました。

# トレンドを把握

最近は以下の様な感じかと思います。

* もちろんレスポンシブデザイン
* トップページは縦長でスクロールさせる
* トップページに大きめなバックグライドイメージ(写真)
* ページ遷移は少なめに
* メニューリンク先はトップページのスクロース先も含める
* より詳細な情報提供をしたいものは個別ページヘ遷移

# 採用したWordPressテーマ

zerif-lite というフリーのテーマです。機能を足すと有料になります。

https://themeisle.com/themes/zerif-lite/

特に色目とかいじらずにそのまま使っています。

<!-- more -->

# 改善ポイント

* 会社ページあくまで自社のスタンスの提示とサービスへのリンクの役割
  * 総情報量は少なく
  * ページ遷移は少なく
* サービス説明用の Portfolio ページはなくす
* サービスの説明はワンフレーズにして、リンク先はサービスサイトへ誘導

総ページ数を少なくできたし、コンテンツの整理もできて、スッキリした。

トップページの全体ビュー

<img src="/images/2015/03/20150316-full-shakesoul-web.png" width="240" alt="ShakeSoul web site full">

# やった作業

今回、WordPressテーマ更新以外にサーバの移設もしました。

多分日本で最も安いVPSだったお名前.comのVPSから [さくらのクラウド](http://cloud.sakura.ad.jp/) に変えています。
さくらのクラウドは [シェイクソウル](http://www.shakesoul.net) のパートナーでもあり、今DevOps的なシステムを一緒に開発しているので、近しい関係でもあるしコストメリットもあるので移ることにしました。

でも、ただ移るだけではサーバインストール・セットアップの手間が都度発生してしまいます。
会社Webサイトのインストールセットアップは [Chef](http://chef.io) で自動化していますが、細かいところは結局 ssh ログインして作業している部分もあり、何よりもMySQLダンプとインポートが必要なのでそれなりに時間はかかっています。

なので、現状動いているものをそのままの状態で移設できることがベストです。

ということで、[Docker](http://docker.com) コンテナにセットアップしてそれをさくらのクラウドのサーバ上で動かしています。
これでスケールアップ時に簡単に移設できます。


ここらへんの手順は需要が大きそうだったら別ポストで書きたいと思います。
