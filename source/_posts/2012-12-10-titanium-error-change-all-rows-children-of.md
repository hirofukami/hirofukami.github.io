---
title: 'Titanium で困ったエラー Error : Change all rows children of TableView'
date: Mon, 10 Dec 2012 05:53:00 +0000
author: Hiro Fukami
layout: post
permalink: /2012/12/10/titanium-error-change-all-rows-children-of/
tumblr_hirofukami_permalink:
  - http://hirofukami.tumblr.com/post/37621600008/titanium-error-change-all-rows-children-of
  - http://hirofukami.tumblr.com/post/37621600008/titanium-error-change-all-rows-children-of
  - http://hirofukami.tumblr.com/post/37621600008/titanium-error-change-all-rows-children-of
tumblr_hirofukami_id:
  - 37621600008
  - 37621600008
  - 37621600008
original_post_id:
  - 38
  - 38
  - 38
categories:
  - Tech
tags:
  - bug
  - coding
  - dev
  - develop
  - error
  - mobile app
  - programing
  - titanium
---
TableViewRow がいくつかある状態で、例えば

*   最初はデフォルト画像を表示させて
*   後から全ての TableViewRow に add してある imageView.image を違う画像に差し替える

とかやりたい時に必ずエラーになってしまう。

imageView.image にアクセスするのが、rows[n].getChildren()[3].image だとして  
2番目の tableViewRow だけを差し替える場合、

<pre><code class="prettyprint linenums">setTimeout(function(){
    tableView.data[0].rows[1].getChildren()[3].image = 'http://graph.facebook.com/1282639350/picture?type=square';
}, 3000);
</code></pre>

これはちゃんと動く。

全ての tableViewRow を対象にしたいので、for で回して、

<pre><code class="prettyprint linenums">setTimeout(function(){
    for (var i = 0; i &lt; data.length; i++) {
        tableView.data[0].rows[i].getChildren()[3].image = 'http://graph.facebook.com/1282639350/picture?type=square';
    }
}, 3000);
</code></pre>

これはエラーになってしまう。

Titanium Studio のエラーメッセージ

    [ERROR] Script Error = 'undefined' is not an object (evaluating 'tableView.data[0].rows[i].getChildren()[3].image = 'http://graph.facebook.com/1282639350/picture?type=square'') at list_friend.js (line 252).
    

多分、`rows[i]` のような数字以外の変数を入れてしまうと認識できないみたい。

フォーラムにも同じようなポストがあって、まだ解決できていないようだ。

*   <a href="http://developer.appcelerator.com/question/113381/looping-through-tableview-to-toggle-selected-row" target="_blank">http://developer.appcelerator.com/question/113381/looping-through-tableview-to-toggle-selected-row</a>
*   <a href="http://developer.appcelerator.com/question/56151/changing-all-rows-in-a-tableview" target="_blank">http://developer.appcelerator.com/question/56151/changing-all-rows-in-a-tableview</a>

どなたか解決方法を知ってらっしゃたら教えてくださいませ。