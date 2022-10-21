---
layout: post
title: "WordPress to Octopress 日本語パーマリンクを直す"
date: 2015-02-15 20:00:00 +0900
comments: true
categories: Tool
---
[WordPressからOctopressにきれいに移行する方法を紹介した][post]が、ちょっとした問題が残っていて、それを解消したので紹介。

## パーマリンクの扱い

[post]: /2014/12/01/right-way-wordpress-to-octopress/

パーマリンクはWordPressの管理画面のパーマリンク設定でカスタマイズできるので、

`/%year%/%monthnum%/%day%/%postname%.html`

として設定していました。

それを [紹介した方法で][post] やると以下の様な markdown ファイルが生成されました。

```
---
title: 仕事で本当に大切にしたいこと
date: Mon, 26 Jul 2004 15:00:00 +0000
author: Hiro Fukami
layout: post
permalink: /2004/07/26/%e4%bb%95%e4%ba%8b%e3%81%a7%e6%9c%ac%e5%bd%93%e3%81%ab%e5%a4%a7%e5%88%87%e3%81%ab%e3%81%97%e3%81%9f%e3%81%84%e3%81%93%e3%81%a8/
tumblr_hirofukami_permalink:
  - http://hirofukami.tumblr.com/post/29542403177
tumblr_hirofukami_id:
  - 29542403177
original_post_id:
  - 741
categories:
  - Book
tags:
  - book
  - reivew
---
```

WordPressパーマリンクでポスト名を日本語で書くと、Octopressのヘッダ内、`permalink`にURLエンコードされた文字列が並びます。

## 問題点

この`permalink`をOctopress上でみると、アーカイブやポスト一覧には並びますが、個別ポストのURLは `/2004/07/26/仕事で本当に大切にしたいこと/` になり、個別ポストのリンクを開くと真っ白になってしまいます。

どうやら、正しく Octopress 上でエンコード・デコードの変換ができないまま html を生成しているみたいです。

## とりあえずさくっと解決する方法

せっかくリンクは作れているのに個別ポストが見れないのはよろしくないので、手っ取り早く解決したいところ。

* まずは個別ポストが見れるように、パーマリンクに日本語エンコードした文字列はやめて英語に変更する
* permalink にポスト名を入れるこだわりを捨てる

1ポストずつ日本語URLエンコードされている部分を変更するのも手間なので、複数ファイル同時に置換した。

テキストエディターは [Sublimi Text][] を使っているので、その置換方法を紹介します。

[Sublimi Text]: http://www.sublimetext.com/

`Shift + Command + F` の全ファイル置換設定画面を出して、正規表現を有効にして以下のように入力。

![Sublime Text Screen Shot](/images/2015/02/20150215-sublime-text-screenshot.png)

* `^(permalink: /..../../..)/%(..).*/$` のように日付の部分までマッチさせたものを格納してReplaceの$1で使う
* 同じ日付のポストがあっても被りづらいようにエンコード1文字目を格納してReplaceの$2で使う
    * 同じ日付で複数ポストしていないのなら、$2 の部分は不要
* source/_posts/ 配下の全ファイルに対して

Replace ボタンを押すと置換されます。

先ほどのポストの permalink の部分は以下のように変わります。

`permalink: /2004/07/26/post-e4/`

あとは、`rake generate && rake preview` で確認して、`rake deploy` でサイトに反映。


ということでした。
