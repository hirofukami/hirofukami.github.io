---
layout: post
title: "Facebook API ページの無期限 Access Token 取得方法"
date: 2017-06-30 23:00:00 +0900
comments: true
image: /images/2017/06/20170630-access_token_never.png
categories:
  - Tech
tags:
  - Facebook
  - API
  - Page
  - Access Token
description: とにかく分かりづらかったので、解消した時の達成感のエネルギーそのままにマニュアルを作ることにした。
---
![Never Access Token](/images/2017/06/20170630-access_token_never.png)


[Players1st][]の開発で、選手の作ったレコードが登録されたら [Players1stのFacebookページ][]に自動ポストする機能を作ろうと思ったのだけど、無期限の Access Token の取得方法がわからず、かなり手間取ってしまった。

とにかく分かりづらかったので、今後の自分や同じような悩みを持った人のためにもマニュアルを残しておく。

参考にさせてもらった手順は以下。

参考URL : [https://gist.github.com/xl1/fe779a817a9d4938193d](https://gist.github.com/xl1/fe779a817a9d4938193d)

もう少し情報を足したほうが迷いなく実施できると思ったので、マニュアルに近い形にまで落とし込んでみたい。
細かい部分は追いきれていないけど、とりあえず手順通りに実施すれば目的は達成できることを目指す。

<!-- more -->

# 事前知識

前もって理解しておくとスムーズに進められる。

## Facebook API 経由でFacebookページを操作するために必要なもの

1. App ID
1. App Secret
1. Page Access Token

App ID, App Secret は [https://developers.facebook.com/apps/](https://developers.facebook.com/apps/) でAppを作成することで取得できる。

## Access Token について

1. User Access Token : 個人の情報をコントロールする
1. App Access Token
1. Page Access Token : Facebookページをコントロールする

の3種がある。

元となるのは User Access Token で、これから Page Access Token を取得する流れになる。

## 使うツール

以下2つを使う。

* [Graph API Explorer][] : Access Token を発行してくれる。
* [Access Token Debugger][] : 発行された Access Token の元となるユーザー、App や期限などが確認できる。

# 手順

1. User Access Token を取得する
1. 長い期限の User Access Token を取得する
1. 長い期限の User Access Token を元に無期限の Page Access Token を取得する

# 1. User Access Token を取得する

1. [Graph API Explorer][] にアクセスする
1. Application をコントロールしたいFacebookページに選択する
1. 「Get Token」ボタンをクリックして、「Get User Access Token」をクリックする

![Graph API Explorer Short](/images/2017/06/20170630-api_explorer_button.png)

するとどのアクセス権を付与したいか設定する画面が表示されるので、
「manage_pages」「publish_pages」の2つにチェックを入れて「Get Access Token」ボタンを押す。

![Permissions](/images/2017/06/20170630-api_explorer_page.png)

そして、Access Token が生成され、Access Token のフォームにある長い文字列がFacebookページとパーミッションを持つ Access Token になる。

![1st Access Token](/images/2017/06/20170630-access_token_1st.png)

## 確認する

現時点では確認は必要ないけどとりあえず見てみる。

[Access Token Debugger][] で有効期限を確認してみる。

1. 先程生成された Access Token をコピーする。
1. [Access Token Debugger][] を別タブで開いて、上部のフォームにペーストする。
1. 「Debug」ボタンを押す。
1. 「Expires」の欄を確認する。

「1498989600 (in about an hour)」の表示が見える。
1時間しか有効期限がないことが分かる。

![short expires](/images/2017/06/20170630-access_token_short.png)

このように、通常 Access Token を生成すると期限付きになってしまう。サービスで使うには無期限の Access Token が必要なので、今得た Access Token を元に生成していく。

# 2. 長い期限の User Access Token を取得する

[Graph API Explorer][] に戻りURLのフォーム欄に以下のパラメーターを加えたURLを入力する。

## 入力するURLパラメーター

`oauth/access_token` に対して4つのパラメーターを渡します。

* grant_type=fb_exchange_toke
* client_id : App ID を入力
* client_secret : App Secret を入力
* fb_exchange_toke : 先程生成した短い期限の Access Token

## 実際入力するURL

URL入力フォームで、GET /v2.9/ まではそのままに、それ以降は以下のように入力する。各パラメーターの値は自分のものに置き換えてください。

`oauth/access_token?grant_type=fb_exchange_token&client_id={app_id}&client_secret={app_secret}&fb_exchange_token={access_token}`

「Submit」ボタンを押します。
うまくいくと、以下のスクリーンショットのようにJSONの返り値の中に「aaccess_token」があります。
この「access_token」の値が長い期限の User Access Token になります。

![long Access Token](/images/2017/06/20170630-api_explorer_get.png)

# 3. 長い期限の User Access Token を元に無期限の Page Access Token を取得する

1. [Graph API Explorer]画面のJSONの中の `access_token` の値部分の access token(長い期限の User Access Token)をコピーする。
1. 「Access Token」のフォームにペーストする。
1. URLを `/me/accounts/` と入力する。
1. 「Submit」ボタンを押す。

するとJSONの返り値が以下のようになる。

![Permissions](/images/2017/06/20170630-api_explorer_pages.png)

自分のユーザーの持つページがそれぞれ表示されており、その中に `access_token` が含まれています。
この値が求めていたページの無期限 Access Token になります。

## 確認する

1. JSON内の該当ページ(私の場合、 `name` が Players1st)の `access_token` の値部分をコピーする。
1. [Access Token Debugger][] を別タブで開いて、上部のフォームにペーストする。
1. 「Debug」ボタンを押す。
1. 「Expires」の欄を確認する。

すると以下のように「Expires」の部分が「Never」になっています。

![Never Access Token](/images/2017/06/20170630-access_token_never.png)

この Access Token は期限なく利用できるものであることが確認できました。

<hr>

おめでとうございます！これでようやくFacebookページの無期限 Access Token が取得できました。

んー、やはりめんどくさい。もうちょっと良い方法にしてほしいところ。
もっと簡単にできるよ、などの情報があれば教えていただければです。

では、Enjoy for developing !

[Players1st]: https://players1.st/
[Players1stのFacebookページ]: https://www.facebook.com/players1st.web/
[Graph API Explorer]: https://developers.facebook.com/tools/explorer
[Access Token Debugger]: https://developers.facebook.com/tools/debug/accesstoken/
