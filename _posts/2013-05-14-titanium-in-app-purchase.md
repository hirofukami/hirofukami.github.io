---
title: Titanium で In App Purchase する手順(StoreKit)
date: Tue, 14 May 2013 16:00:19 +0000
author: Hiro Fukami
layout: post
permalink: /2013/05/14/titanium-in-app-purchase/
categories:
  - Tech
tags:
  - CopytoTumblr
  - develop
  - In-App Purchase
  - manual
  - mobile app
  - titanium
---
Apple の審査も <a href="https://itunes.apple.com/jp/app/dm-chat/id447521974" target="_blank">DmAtChat</a> と <a href="https://itunes.apple.com/jp/app/ftupdater-facebook-twitterniposutosurudakeno/id448808656?mt=8" target="_blank">FTupdater</a> 両方通ったことだし、Titanium Mobile 上で In-App Purchase を導入する手順をまとめておく。

基本的には4つの環境にまたがって実現する。

1.  iTunes Connect
2.  コーディング、Titanium Studio
3.  Xcode
4.  iPhone実機

参考にしたURL

*   http://kray.jp/blog/iphone-in-app-purchase/
*   http://d.hatena.ne.jp/glass-_-onion/20111201/1322697417

前提

*   すでに iTunes Connect 上でアプリが登録されていること
*   開発環境に Titanium Studio でプロジェクトが作成され、iPhone シミュレータが起動できること
*   開発環境に Xcodeがインストールされていること

## iTunes Connect 上での作業

### In-App Purchase の Product を作る

1.  iTunes Connect へログイン、&#8221;Manage Your Apps&#8221; をクリック、In-App Purchase を導入したいAppをクリック
2.  &#8220;Manage In-App Purchases&#8221; をクリック
3.  &#8220;Create New&#8221; をクリック
4.  Select Type にて選択する 
    *   消費する一時的なアイテムなら Consumable
    *   ある機能を有効にするなら Non-Consumable
    *   定期購読系なら各 Subscription : 審査基準が厳しいなどあるようでまだ試したことはない
5.  プロダクト情報を入力する 
    1.  Reference Name : 内部管理用の表示名
    2.  Product ID : 内部管理用ID、他の App も含めてユニークにする必用があるので、&#8221;AppName_ProductName&#8221; みたいにするといいかも
    3.  価格を決める
    4.  Detail部分を入力する 
        1.  Language : デフォルトだと英語。Display Name は App/App Sore 内で表示される名前なので、ユーザにわかりやすいものにする
        2.  Hosting Content with Apple : Apple の中にコンテンツを預けていなければ &#8220;No&#8221;。通常 No でいいはず
        3.  Screenshot for Review : Appleが審査するときのため用のスクリーンショットで App Store内には表示されない。何かしらアップロードしないと Status が変わらずプロダクト登録完了しないので、適当なものをあげておいて、動作確認後取ったスクリーンショットをAppサブミット前に変更する。
    5.  &#8220;Save&#8221; をクリック
6.  In-App Purchases の一覧表にて、Status が Ready to Submit になったらOK
7.  タイプが Auto-renew subscriptions の場合、一覧表の下にある View or generate shared secret のリンクを押す、キーをメモっておく

### iTunes Connect テストユーザを作る

1.  iTunes Connect のトップページに戻り、Manage Users > Test User > Add New User して作成する
2.  クレジットカードの情報はいらない。入力するとテスト時に購入処理ができない。登録時に必須になっている場合は一度登録してあとで削除する 
    *   参考QA http://iphonedevsdk.com/forum/iphone-sdk-development/63008-in-app-purchase-test-account-verification-required-cant-get-passed.html

## StoreKit をプロジェクトに配置する

1.  Appcelerator Marketplace で StoreKit module のページを開く 
    *   https://marketplace.appcelerator.com/apps/794
2.  Download してPCローカルに保存された zip ファイルを解凍する
3.  In App Purchase を適用したいプロジェクトのディレクトリに StoreKit module を配置する 
    *   iPhone用なのでパスは、ProjectName/modules/iphone/ti.storekit となる
4.  Titanium Studio にて該当のプロジェクトの tiapp.xml ファイルにて確認する 
    *   Modules の一覧表に &#8220;ti.sorekit&#8221; が確認出来ればOK
    *   XMLファイルのソース的には <modules>タブの中に以下の様な記述があればOK 
        <pre>&lt;module platform="iphone" version="1.6.4"&gt;ti.storekit&lt;/module&gt;</pre>

## コードを書く

<!--more-->example の app.js を読んで、記述の仕方を把握する

*   ProjectName/modules/iphone/ti.storekit/versionNo/example/app.js
*   基本的にはここに書かれている処理を理解すれば流用可能

基本的な機能

購入情報は iPhone のローカルメモリと iTunes Connect に保存されるが、ローカルメモリ上に情報がない場合(購入済みなのに購入していないことになっている)ことを復旧させるためにリストア(iTunes Connect の購入履歴をローカルメモリへコピーする)処理がある。

*   Storekit.canMakePayments : 購入可能状態かどうか、true/false で返る
*   Storekit.requestProducts(product) : iTunes Connect で作成した In App Purchase プロダクト(product)の情報を得る
*   Storekit.purchase(product) : product を購入する
*   Storekit.restoreCompletedTransactions() : すでに購入済みのプロダクトがローカルにない時にリストアする

最初に記述する部分

<pre class="brush: jscript; title: ; notranslate" title="">var Storekit = require('ti.storekit');

Storekit.receiptVerificationSandbox = true;
</pre>

In App Purchase のタイプが Auto-renew subscriptions の場合、receiptVerificationSharedSecret の記述をする。メモっておいた Shared Secret のキーを記述する。

<pre class="brush: jscript; title: ; notranslate" title="">Storekit.receiptVerificationSharedSecret = "&lt;YOUR STOREKIT SHARED SECRET HERE&gt;";
</pre>

Expample app.js 上で function にして記述してあるので、そのまま流用しやすくなっている。

<pre class="brush: jscript; title: ; notranslate" title="">function requestProduct(identifier, success)
{
	showLoading();
	Storekit.requestProducts([identifier], function (evt) {
		hideLoading();
		if (!evt.success) {
			alert('ERROR: We failed to talk to Apple!');
		}
		else if (evt.invalid) {
			alert('ERROR: We requested an invalid product!');
		}
		else {
			success(evt.products[0]);
		}
	});
}
</pre>

購入処理のイベントが拾えて購入済、購入処理、リストア処理、Fail ごとに処理も書かれているので、これもそのまま流用できる。

<pre class="brush: jscript; title: ; notranslate" title="">Storekit.addEventListener('transactionState', function (evt) {
	hideLoading();
	switch (evt.state) {
		case Storekit.FAILED:
			if (evt.cancelled) {
				alert('Purchase cancelled');
			} else {
				alert('ERROR: Buying failed! ' + evt.message);
			}
			break;
		case Storekit.PURCHASED:
			if (verifyingReceipts) {
				Storekit.verifyReceipt(evt, function (e) {
					if (e.success) {
						if (e.valid) {
							alert('Thanks! Receipt Verified');
							markProductAsPurchased(evt.productIdentifier);
						} else {
							alert('Sorry. Receipt is invalid');
						}
					} else {
						alert(e.message);
					}
				});
			} else {
				alert('Thanks!');
				markProductAsPurchased(evt.productIdentifier);
			}
			break;
		case Storekit.PURCHASING:
			Ti.API.info("Purchasing " + evt.productIdentifier);
			break;
		case Storekit.RESTORED:
			// The complete list of restored products is sent with the `restoredCompletedTransactions` event
			Ti.API.info("Restored " + evt.productIdentifier);
		    break;
	}
});
</pre>

## テスト

Titanium Studio 上から iPhone シミュレーターを起動するとテストの購入処理ができないようで、Xcode から実機で起動してのテスト方法が Example app.js の中で書いてある。

> 1) Storekit works in two different environments: &#8220;Live&#8221; and &#8220;Sandbox&#8221;. It automatically uses the &#8220;Sandbox&#8221; when you run your app in Xcode. This means that &#8220;Deploy to Device&#8221; in Titanium Studio will connect to the &#8220;Live&#8221; environment! Using a test account in &#8220;Live&#8221; will ruin the test account. And paying for items with a live account will quickly suck up your hard earned cash. So be careful!

1.  テストを実行する実機の iPhone 上で Settings > iTunes & App Stores でログアウトしておく
2.  1回、該当のプロジェクトを Titanium Studio から iPhone シミュレーターで起動して、落とす
3.  プロジェクト内にある、ProjectName/build/iphone/ProjectName.xcodeproj を開く
4.  Xcode が起動する
5.  実機の iPhone をつないで、&#8221;Scheme&#8221; でその実機を選ぶ
6.  &#8220;Run&#8221; ボタンを押して、実機からアプリが起動して購入処理をテストする
7.  iTunes Connect で作成したテストユーザの情報でログインする
8.  購入が出来ればOK

[<img class="size-medium wp-image-919 alignnone" alt="iOS Simulator Screen shot 2013.04.09 10.35.29" src="/images/2013/05/ios-simulator-screen-shot-2013-04-09-10-35-29.png?resize=169%2C300" data-recalc-dims="1" />][1][  <img class="size-medium wp-image-920 alignnone" alt="IMG_2464" src="/images/2013/05/img_2464.png?resize=169%2C300" data-recalc-dims="1" />][2]  [<img class="size-medium wp-image-921 alignnone" alt="IMG_2465" src="/images/2013/05/img_2465.png?resize=169%2C300" data-recalc-dims="1" />][3]

## 

## Apple にサブミットして審査を受ける

iTunes Connect の Manage Your Apps にて新しいバージョンのアプリを作る。その際に In App Purchase を適用することを忘れずに。

UI上 Restore のボタンを用意するには必須。処理上で書いていてもUI上にリストアボタンがないと Apple が審査時にリジェクトする。

##

 [1]: /images/2013/05/ios-simulator-screen-shot-2013-04-09-10-35-29.png
 [2]: /images/2013/05/img_2464.png
 [3]: /images/2013/05/img_2465.png