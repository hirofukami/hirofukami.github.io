---
title: Titanium SDK 3.1.0 対応で色々大変だったというメモ
date: Fri, 17 May 2013 11:02:27 +0000
author: Hiro Fukami
layout: post
permalink: /2013/05/17/titanium-sdk-3-1-0-trouble/
categories:
  - Tech
tags:
  - apps
  - develop
  - Morning Latte
  - titanium
  - trouble
---
Titanium SDK/Studio 3.1.0 に対応した <a href="https://itunes.apple.com/jp/app/morning-latte/id585111783?mt=8" target="_blank">Morning Latte</a> がリリースできたので、対応するために色々大変だったメモを残しておく。

今までのコードをそのまま Titanium SDK 3.1.0 で動かすとエラーやワーニングでてまともに動かなかったので、色々対応が必要だった。Titanium3.1.0 のリリース内容は <a href="http://developer.appcelerator.com/blog/2013/04/titanium-sdkstudio-3-1-0-release-candidate-now-available.html" target="_blank">Appcelerator Blog でも紹介されいてる</a>通りで、その中で <a href="https://itunes.apple.com/jp/app/morning-latte/id585111783?mt=8" target="_blank">Morning Latte</a> に関係しそうだったのは、

*   iOS Retina simulator Support
*   Facebook V3 モジュール提供

の2つ。それ以外に対応が必要だったのは、

*   そもそも Titanium Studio 3.1.0 へのアップデートが出来ない
*   せっかく設定した fb module で Facebook アプリ認証をさせると、他のページで fb.module 読むとセッションエラーが出る
*   tabGroup を close() しようとするとワーニングが出た後、アプリがクラッシュして落ちる

とかとか、トラブルになってしまいこれを解消する必要があった。行った順に1つ1つ解消メモを残しておく。

## Titanium Studio 3.1.0 にアップデート出来ない

のっけからつまずいてしまったのはこれ。Titanium Studio から、Help > Check for Updates すると、レポジトリエラーのメッセージが出て処理が止まってしまう。

上書きインストールを試みるために、Titanium Studio の<a href="https://my.appcelerator.com/resources" target="_blank">ダウンロードサイト</a>からファイルをダウンロードして、Mac の Applications > Titanium Studio に保存して起動してみると、エラーが出て起動できなくなってしまった。

結局解決した方法は、

1.  AppCleaner.app などのアンインストールツールを使って一度 Titanium Studio をアンインストールする 
    *   Mac にはアプリアンインストールツールがないと思うので、こういったツールをインストールする必要がある
    *   Applications の中には Titanium のディレクトリがなくなる
2.  Titanium Studio 3.1.0 を新規にインストール、Applications に Titanium Studio ディレクトリをコピー
3.  起動する
4.  最初にプロジェクト用のディレクトリを指定するので今までのプロジェクトがあるパスを設定する
5.  プロジェクト内にある tiapp.xml をちゃんと読んでくれるので、特にこれ以上の設定は必要なく今までどおり使える

結局、必要な設定情報は tiapp.xml にあるので、あまり悩まずアンインストールしてしまってよかったというお話。今後にも使えそうなティップス。

## Facebook V3 モジュール対応

既存の Titanium.Facebook が使えなくなって、モジュールの一つとして位置づけられ Modules.Facebook  になりました。ということなので記述的には、

いままで

<pre class="brush: jscript; title: ; notranslate" title="">Ti.Facebook.appid = &lt;Your App ID&gt;;
Ti.Facebook.permissions = ['publish_stream', 'read_stream', 'user_likes'];

Ti.Facebook.requestWithGraphPath('me/likes', {}, &quot;GET&quot;, function(r) {....
</pre>

のように書いていたものを、

<pre class="brush: jscript; title: ; notranslate" title="">var fb = require('facebook');

fb.appid = &lt;Your App ID&gt;;
fb.permissions = ['read_stream', 'user_likes', 'publish_stream'];
fb.forceDialogAuth = true;
fb.authorize();

fb.requestWithGraphPath('me/likes', {}, &quot;GET&quot;, function(r) {....
</pre>

このように基本 Ti.Facebook. を fb. に置き換えればOK。

<!--more-->

### fb.forceDialogAuth は true でないと使えない

Facebook module 3.1 に対応したことで、Facebook アプリがインストールされているiOS6の端末ではFacebookアプリが起動して認証情報を渡してログイン処理を省略できる機能が使えるようになった(多分)。

それを有効にするには、

<pre>fb.forceDialogAuth = false;</pre>

と記述する。

しかし、これには条件があって、permission が READ系しか最初に定義できない。APIを叩くときのGETのみが設定できて read\_stream や user\_likes などが使える。じゃあPOST系の publish_stream とかどうするかというと、最初の認証が通った後、

<pre class="notpretty" id="ext-gen1555">fb.reauthorize()</pre>

<p class="notpretty">
  を使って、POST系の permission を再認証する必要がある。reauthorize を呼んだ時点でまた Facebook アプリが起動して戻ってくる遷移をするので、UX的にはよろしくないなぁと思う。
</p>

百歩譲って、POST系 permission は使わないからやってみようとすると、画面遷移の仕方によってはすぐセッションエラーが出てしまう。

実際自分が行なって遭遇した流れは、

1.  TOPページ、fb.authrize(); 済
2.  Setting ページで、fb.logout() して fb.authorize()、認証された
3.  TOPページに戻る(Settingページは閉じられる)
4.  Friendのフィードリストページを開いて、各フィードをタップすると個別フィードページが開く
5.  個別フィードでは like や comment もできるので、fb module を読んでいる
6.  個別フィードが開いた時点で、 <pre class="default prettyprint prettyprinted"><code>&lt;span class="str">"FBSession: should only be used from a single thread"&lt;/span></code></pre>
    
    が出て読み込めなくなる</li> </ol> 
    多分、セキュリティ的観点から認証したページからしか認めないようになっているのだろうけど、認証したページとその後 fb module を呼び出したいページが異なる場合はどうやって渡せばいいのか、色々試したが解消できなかった。結局 fb.forceDialogAuth = true; としてこの機能自体使わないようにしている。POST系の permission も最初から定義できるしメリットはあるので、これはこれで良しとした。
    
## iOS Retina simulator Support
    
iPhone simulator を起動すると今まで iPhone Retina 4-inch で起動していたものが、iPhone (非Retina 3.5-inch) で起動してきてしまった。iPhone simulator 上の Hardware で変更すると simulator 自体が終了してしまうので、困ったなぁと思ったら Titanium Studio 上で指定できるようになったので、デフォルトの設定を変える必要があった。
    
1.  Titanium Studio > Preferences より、左のメニューから Studio > Platforms > iOS を選択
2.  Default Display から Retina & Tall を選択
3.  Apply ボタンを押して、OK を押して閉じる
    
[<img class="aligncenter size-medium wp-image-963" alt="Screen Shot 2013-05-17 at 10.21.43" src="/images/2013/05/screen-shot-2013-05-17-at-10-21-43.png?resize=300%2C132" data-recalc-dims="1" />][1]
    
## いきなり tabGroup.close() するとクラッシュする
    
これがアップデート情報になく解消までに時間がかかってしまった点だかが、tabGroup を閉じる処理はaddされているtabがない状態で行う。<a href="https://itunes.apple.com/jp/app/morning-latte/id585111783?mt=8" target="_blank">Morning Latte</a> が行なっている処理は、フィードのリストページからあるフィードのViewをタップすると新たにtabGroupページが下からニョキッと出てくる。出てきたページで個別のフィードはtabGroupになっているので、写真、リンク先、Likeした人のリストなどの画面に移る。
    
[<img class="size-medium wp-image-964 alignnone" alt="iOS Simulator Screen shot 2013.05.17 10.01.37" src="/images/2013/05/ios-simulator-screen-shot-2013-05-17-10-01-37.png?resize=169%2C300" data-recalc-dims="1" />][2] =>  [<img class="size-medium wp-image-965 alignnone" alt="iOS Simulator Screen shot 2013.05.17 10.02.54" src="/images/2013/05/ios-simulator-screen-shot-2013-05-17-10-02-54.png?resize=169%2C300" data-recalc-dims="1" />][3] =>  [<img class="size-medium wp-image-966 alignnone" alt="iOS Simulator Screen shot 2013.05.17 10.01.41" src="/images/2013/05/ios-simulator-screen-shot-2013-05-17-10-01-41.png?resize=169%2C300" data-recalc-dims="1" />][4] => [<img class="size-medium wp-image-967 alignnone" alt="iOS Simulator Screen shot 2013.05.17 10.34.34" src="/images/2013/05/ios-simulator-screen-shot-2013-05-17-10-34-34.png?resize=169%2C300" data-recalc-dims="1" />][5]
    
個別フィードのページの左上ボタンでウインドウを閉じて、フィード一覧のページに戻る。
    
このウインドウを閉じる処理は、
    
<pre class="brush: jscript; title: ; notranslate" title="">closeb.addEventListener('click', function(){
            newtabGroup.close();
        })
</pre>
    
で今まで良かったのが、ワーニングが出た後、クラッシュして落ちてしまうようになった。色々調べたりして結局以下のようにして回避できた。
    
<pre class="brush: jscript; title: ; notranslate" title="">closeb.addEventListener('click', function(){
            newtabGroup.animate({top:600,duration:300});
            setTimeout(function(){
                newtabGroup.removeTab(newtab);
                newtabGroup.close();
            },400);
        });
</pre>
    
アニメーションの部分はなくていいが、結局addされているtabをremovetab()で削除して、空の状態になったtabGroupをclose()する。
    
参考にしたURLは以下、
    
*   Tinitaum ドキュメントにも書いてあった : http://docs.appcelerator.com/titanium/latest/#!/api/Titanium.UI.Tab-method-close
*   フォーラムより : http://developer.appcelerator.com/question/139857/tabgroupclose-doesnt-work-on-android
    
ということで、色々はまった方のお役に立てれば何よりです。

 [1]: /images/2013/05/screen-shot-2013-05-17-at-10-21-43.png
 [2]: /images/2013/05/ios-simulator-screen-shot-2013-05-17-10-01-37.png
 [3]: /images/2013/05/ios-simulator-screen-shot-2013-05-17-10-02-54.png
 [4]: /images/2013/05/ios-simulator-screen-shot-2013-05-17-10-01-41.png
 [5]: /images/2013/05/ios-simulator-screen-shot-2013-05-17-10-34-34.png