---
title: Facebook/Twitterにポストするだけの簡単アプリ-FTupdater v.1.2.0 リリースしました
date: Mon, 05 Aug 2013 09:08:13 +0000
author: Hiro Fukami
layout: post
permalink: /2013/08/05/ftupdater-v120-release/
geo_public:
  - 0
  - 0
categories:
  - Business
tags:
  - apps
  - FTupdater
  - iOS
  - release
---
あまりアプリの更新情報をブログに書いていなかった気がしたので、書いてみることにしました。

実は Titanium を学び始めてすぐ2つのアプリを作ったのですが、目的を絞った機能の少ないアプリをちゃっちゃと出してみようと思って作ったアプリが <a href="https://itunes.apple.com/jp/app/ftupdater/id448808656" target="_blank">FTupdater</a> になります。

FTupdater はFacebook/Twitterにポストするだけのアプリで、ポストする内容によって FB/TW を切り替えたいまたはマルチポストしたい人に使っていただけるだろうと思っています。

今まで細々バージョンを重ねて v.1.1.x 台が更新されていたのですが、ついに？ v.1.2.0 にバージョンアップしました。  
Kii のエンジニアの方に、機能を足した時に v.1.x 台を上げるようにしていると聞いたので真似してみました。

今回足した機能は「Sent Page」で、今までポストした履歴が見れるページになります。  
ポストするだけだと今まで自分がどんなことをポストしたかは、FB や TW の自分のページを後で確認する必要がありました。私だったら時々、<a href="https://www.facebook.com/fukami" target="_blank">自分のFBのプロフィールページ</a>と<a href="https://twitter.com/d_sea" target="_blank">Twitterページ</a>を見返したりします。  
FTupdater でポストした時に確認先が分散されるのは手間だし、シンプル機能に特化するポリシーのアプリですが、これくらいは付けてもいいかと思い足しました。

で、ビューはこんな感じ。

[<img class="wp-image-1119 alignnone" alt="iOS Simulator Screen shot 2013.07.31 19.23.01" src="/images/2013/08/ios-simulator-screen-shot-2013-07-31-19-23-01.png?resize=346%2C614" data-recalc-dims="1" />][1]

実装内容としては、  
アプリ内部の sqlite で保存用のテーブルを作って、時刻、テキスト、画像ファイルパス、ポスト先情報などを保存するようにして、画像を一緒にポストした場合はそのコピーを applicationDataDirectory に保存するのようなことをしています。  
データが溜まりまくるのもなんなので、Edit から削除するようにしています。

実装のことを書くときりがないので、細かいことはさておき、  
パッとポストしたいことを思いついた時にさっと使えるアプリってあまりないと思うので、  
良かったら使ってみてください〜

[<img class="wp-image-1120 alignleft" alt="available-on-the-app-store-icon" src="/images/2013/08/available-on-the-app-store-icon.png?resize=180%2C54" data-recalc-dims="1" />][2]

 [1]: /images/2013/08/ios-simulator-screen-shot-2013-07-31-19-23-01.png
 [2]: https://itunes.apple.com/jp/app/ftupdater/id448808656