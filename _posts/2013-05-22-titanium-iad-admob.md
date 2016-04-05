---
title: Titanium で iAd/AdMob を効率よく表示する
date: Wed, 22 May 2013 08:51:59 +0000
author: Hiro Fukami
layout: post
permalink: /2013/05/22/titanium-iad-admob/
categories:
  - Tech
tags:
  - AD
  - AdMob
  - apps
  - develop
  - iAd
  - titanium
---
アプリの広告表示で iAd と Admob の実測値を<a title="iAd と AdMob の実測比較結果" href="http://hirofukami.com/2013/03/19/iad-vs-admob/" target="_blank">過去のエントリーで紹介した</a>が、その結論として、

*   CPM は iAd の方が高いので優先的に表示
*   でも、iAd の Fill Rate が悪すぎるので、表示エラーになった時に CPM が低いけど Fill Rate ほぼ100％の AdMob を表示する

とまとめた。

実際、Titanium でどうやっているのか紹介しておく。参考になれば幸いです。

iAd を表示するには Titanium に標準でついてくる Titanium.UI.iOS.AdView を使う。AdMob は Appcelerator 提供の Admob module を利用する。<a title="Titanium Mobile で開発した iPhone アプリに AdMob を導入する" href="http://hirofukami.com/2012/01/23/titanium-mobile-iphone-admob/" target="_blank">Admob moudle のセットアップは以前書いたエントリー</a>を参照ください。

app.js に表示するサンプルコードを示します。

<pre class="brush: jscript; title: app.js; notranslate" title="app.js">///////////// iAD ///////////////
var win = Ti.UI.currentWindow;

var IadView = require('ui/handheld/iadView');
var iad = new IadView();
var IadErrorView = require('ui/handheld/iadErrorView');
var errorview = new IadErrorView();
iad.addEventListener('error', function(e){
    iad.hide();
    Ti.API.debug('iAd load Error: ' + e.message);
});
iad.addEventListener('load', function(){
    iad.show();
    Ti.API.debug('iAd load OK');
});
win.add(iad);
iad.hide();
win.add(errorview);
</pre>

iad は iAd、errorview は AdMob を表示することで、それぞれ win.add(iad), win.add(errorview) で currentWindow に表示させる。  
iad と errorview は上下に重なっている状態にして、iad を隠したり表示したりする。iad が隠された時に errorview が表示される。という仕組み。  
iad の error イベント(提供エリア外 or 広告在庫がなくて表示ができない)場合に iad.hide() で隠す。  
逆に load イベント(表示できた) 時は iad.show() で表示させる。

<pre class="brush: jscript; title: iadView.js; notranslate" title="iadView.js">// iAD
function iadView(){
    var iadview = Ti.UI.iOS.createAdView({
       top : 0,
       left : 0,
       height : 50,
       width : 320,
       zIndex : 3
    });
    return iadview;
};

module.exports = iadView;
</pre>

<!--more-->

iAd の表示設定はこんな感じ。zIndex : 3 にしてある。画面上部に表示させるので top : 0 にしてある。  
広告のサイズにあわせて、height : 50, width : 320 としている。

<pre class="brush: jscript; title: iadErrorView.js; notranslate" title="iadErrorView.js">// Admob
function admobView(){
    var Admob = require('ti.admob');
    var admobview = Admob.createView({
        top: 0,
        left: 0,
        width: 320,
        height: 50,
        zIndex : 1,
        adBackgroundColor: 'black',
        keywords: '',
        publisherId: &lt;Your API Key&gt; // Replace this string with your own API key!
    });
    return admobview;
}

module.exports = admobView;
</pre>

Admob の表示設定はこんな感じ。こちらは zIndex : 1 にしていて、zIndex : 3 を設定した iadView の方が上の面にくるところがミソ。  
publisherId の部分は自分で取得したIDを入れてください。

これを動かすと、iad &#8216;load&#8217; の時は

[<img class="aligncenter size-medium wp-image-989" alt="iOS Simulator Screen shot 2013.05.22 8.42.36" src="/images/2013/05/ios-simulator-screen-shot-2013-05-22-8-42-36.png?resize=169%2C300" data-recalc-dims="1" />][1]

iad &#8216;error&#8217; の時は

[<img class="aligncenter size-medium wp-image-990" alt="iOS Simulator Screen shot 2013.05.22 8.44.22" src="/images/2013/05/ios-simulator-screen-shot-2013-05-22-8-44-22.png?resize=169%2C300" data-recalc-dims="1" />][2]という感じに表示されましたと。

これらの設定は、私が作ったアプリだとメンションとDMだけのやり取りに特化したTwitterクライアント <a href="https://itunes.apple.com/jp/app/dm-chat/id447521974?mt=8" target="_blank">DmAtChat</a>、FacebookとTwitterに同時ポストできる <a href="https://itunes.apple.com/jp/app/ftupdater-facebook-twitterniposutosurudakeno/id448808656?mt=8" target="_blank">FTupdater</a> で実装されています。

 [1]: /images/2013/05/ios-simulator-screen-shot-2013-05-22-8-42-36.png
 [2]: /images/2013/05/ios-simulator-screen-shot-2013-05-22-8-44-22.png