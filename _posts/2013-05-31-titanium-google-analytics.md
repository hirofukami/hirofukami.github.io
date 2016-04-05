---
title: Titanium で Google Analytics を使う手順
date: Fri, 31 May 2013 11:17:59 +0000
author: Hiro Fukami
layout: post
permalink: /2013/05/31/titanium-google-analytics/
categories:
  - Tech
tags:
  - apps
  - develop
  - Google Analytics
  - titanium
---
ユーザがアプリをどんなふうに利用しているかは、最近ではトラッキングして分析するのが当たり前になっているが、ではどうやって？というところの解は Google Analytics を使うか、MBaaS の分析機能＆イベントと絡ませてデータを投げる(Titanium の場合、基本Analyticsでは情報不足なのでACSのオブジェクト作成をしてデータを貯める)とかが考えられる。

ACS のオブジェクト作成でデータを貯めてみたが、結局集計は自分でやらなくてはいけないのであまりいい運用ができていなかった。ということで、やっぱりお手軽な Google Analytics を入れるかとういことでその手順をまとめる。

Titanium-Google-Analytics というモジュールを適用して、Google Analytics の1つのウェブサイトとしてデータ収集してくれる。ちょっと分かりづらいポイントは Google Analytics for Mobile ではなくて、モジュールはアプリの振る舞いをウェブサイトとして Google Analytics に投げるこをとしてくれるので、Google Analytics 上はあくまでウェブサイトとしての扱いになる。

参考URL

*   http://tech4403.hatenablog.com/entry/20110128/1296189155
*   http://webanalyst.biz/tool/google-analytics/titanium-studio-google-analytics/

ほぼ参考URL通りなのだけど、バージョンも上がって若干異なる記述が必要だったのでまとめておく。

## Google Analytics で ID を取得する

他のIDでも使いまわせるが、アプリのトラッキングデータが多くなったら支障が出るような説明があったので、アカウントから新しく作ることにした。

1.  Google Analytics にログインして、「アナリティクス設定」> 「+新しいアカウント」をクリック
2.  「トラッキングの対象」は「ウェブサイト」を選択する 
    *   気持ち的にはアプリが対象なので「アプリ」を選びたいが、収集できないので注意
3.  ウェブサイト名、サイトURL、アカウント名を適当に入力する 
    *   サイトURLはそのURLでないとNGということはないので、適当なURLでよい
    *   アカウント名 > ウェブサイト名 の階層関係なので、それぞれ管理しやすい名前にする
4.  ID が発行されるのでメモる

## Titanium-Google-Analytics をセットアップする

github で公開されている <a href="https://github.com/rogchap/Titanium-Google-Analytics" target="_blank">Titanium-Google-Analytics</a> を git clone してもいいし、<a href="https://github.com/rogchap/Titanium-Google-Analytics/archive/master.zip" target="_blank">ZIPファイルをダウンロード</a>してファイルを取得する。

いくつかファイルやディレクトリがあるが実際使うのは、Resources/Ti.Google.Analytics.js になる。

使い方は Resources/app.js にサンプルとして書いてあるので、読んでみる。

<pre class="brush: jscript; title: ; notranslate" title="">var gaModule = require('Ti.Google.Analytics');
var analytics = new gaModule('UA-XXXXXX-X');

// Call the next function if you want to reset the analytics to a new first time visit.
// This is useful for development only and should not go into a production app.
//analytics.reset();

// The analytics object functions must be called on app.js otherwise it will loose it's context
Ti.App.addEventListener''analytics_trackPageview', function(e){
    analytics.trackPageview('/iPad' + e.pageUrl);
});

Ti.App.addEventListener('analytics_trackEvent', function(e){
    analytics.trackEvent(e.category, e.action, e.label, e.value);
});

// Starts a new session as long as analytics.enabled = true
// Function takes an integer which is the dispatch interval in seconds
analytics.start(10,true);

// track page view on focus
win1.addEventListener('focus', function(e){
    analytics.trackPageview('/win1');
});
</pre>

require で Ti.Google.Analytics.js を読み込んで、Google Analytics でアカウント作成時に取得した UA から始まる ID を記述する。  
addEventListener でページビューした時とイベント発生時を定義する。trackPageview はページビュー用のデータ収集、trackEvent はボタンを押した時などのイベントと絡めるデータ収集。  
analytics.start(10,true); は10秒間隔でデータを送っている？ようです。  
あとは、ページビューだったら Window が focus されたイベント時に trackPageview を値を添えて呼び出す。のような動作させたい箇所で記述する。

<!--more-->

## 自分のアプリコードに反映する

実際自分のアプリでは以下のように記述している。ちなみにこれは <a href="https://itunes.apple.com/jp/app/ftupdater-facebook-twitterniposutosurudakeno/id448808656?mt=8" target="_blank">Facebook と Twitter に同時にマルチポストするだけのアプリ FTupdater</a> からの抜粋。

<pre class="brush: jscript; title: app.js; notranslate" title="app.js">var gaModule = require('Ti.Google.Analytics');
var analytics = new gaModule('UA-xxxxxxx-1');

Ti.App.addEventListener('analytics_trackPageview', function(e){
    var path = '/ft/' + Titanium.Platform.name;
    analytics.trackPageview(path + e.pageUrl);
});

Ti.App.addEventListener('analytics_trackEvent', function(e){
    analytics.trackEvent(e.category, e.action, e.label, e.value);
});

Ti.App.Analytics = {
    trackPageview:function(pageUrl){
        Ti.App.fireEvent('analytics_trackPageview', {pageUrl:pageUrl});
    },
    trackEvent:function(category, action, label, value){
        Ti.App.fireEvent('analytics_trackEvent', {category:category, action:action, label:label, value:value});
    }
}

analytics.start(10,true);
</pre>

Ti.App.Analytics で trackPageview と trackEvent を定義して、他の js ファイルからも呼び出せるようにしている。

<pre class="brush: jscript; title: update.js; notranslate" title="update.js">var win = Ti.UI.currentWindow;

win.addEventListener('focus', function(e){
    Ti.App.Analytics.trackPageview('/update');
});

doneButton.addEventListener('click', function(e){
    textArea.blur();
    Ti.App.Analytics.trackEvent('Text Input','push done button','doneButton',1);
});
</pre>

ページビューを取りたいところは、Window が focus した時と、doneButton を押した時に trackEvent を呼び出している。trackEvent の値の渡し方だけど、最後の value として「1」を入力している部分にテキストを入れても、Google Analytics 上は「1」と表示されるので、数字以外はダメみたい。

## Google Analytics で確認する

1.  Titanium Studio でシミュレーターを起動する
2.  Google Analytics で該当のサイトをクリックして、左メニューから「リアルタイム」> 「サマリー」をクリックする
3.  シミュレーターのアプリ内で、ページの表示を繰り返したり、イベントラッキングしている処理をしてみたりする 
    *   結構繰り返してしばらくたたないと、データがたまらないのか、Google Analytics には表示されなかった
4.  Google Analytics 上にアクティブユーザ数 1名 とか、アクティブページとかに表示がされればOK

Google Analytics のサマリー

[<img class="aligncenter size-large wp-image-1003" alt="Screen Shot 2013-05-31 at 11.00.44" src="/images/2013/05/screen-shot-2013-05-31-at-11-00-44.png?resize=830%2C355" data-recalc-dims="1" />][1]

コンテンツ(ページビュー)

[<img class="aligncenter size-large wp-image-1004" alt="Screen Shot 2013-05-31 at 11.01.12" src="/images/2013/05/screen-shot-2013-05-31-at-11-01-12.png?resize=830%2C395" data-recalc-dims="1" />][2]

イベント

[<img class="aligncenter size-large wp-image-1005" alt="Screen Shot 2013-05-31 at 11.01.42" src="/images/2013/05/screen-shot-2013-05-31-at-11-01-42.png?resize=830%2C481" data-recalc-dims="1" />][3]

無事動作確認できたら良かったねと。めでたしめでたし。

 [1]: /images/2013/05/screen-shot-2013-05-31-at-11-00-44.png
 [2]: /images/2013/05/screen-shot-2013-05-31-at-11-01-12.png
 [3]: /images/2013/05/screen-shot-2013-05-31-at-11-01-42.png