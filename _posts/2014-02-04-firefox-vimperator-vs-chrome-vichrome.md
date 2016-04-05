---
title: Firefox + Vimperator から Chrome + Vichrome に乗り換えた
date: Tue, 04 Feb 2014 10:24:36 +0000
author: Hiro Fukami
layout: post
permalink: /2014/02/04/firefox-vimperator-vs-chrome-vichrome/
categories:
  - Tool
tags:
  - add-on
  - browser
  - Chrome
  - extensions
  - firefox
  - tool
---
PCの操作上でブラウザを使う時間はかなりを占めるし、あらゆるサービスが使うことができるようになって、ますますその割合は増えていっている。  
ということで、ブラウザ操作を効率的に行うのはパフォーマンスをよく保つのに結構重要だと思っている。

## Vimperaor

[<img src="http://teramako.github.io/doc/browser-workshop-20101127/vimperator-logo.png?w=830" alt="vimperator" data-recalc-dims="1" />][1]

5年前くらいに Vimperator の存在を教えてもらって、それ以来ずっと使いづづけている。

Vimperator は vi のようにあらゆる作業をキーボードで行えてカスタマイズ性も高い。マウス操作に時間が取られずに快適に操作ができていた。

y キーで現在開いているページのURLをコピー、p でコピーしたものをURLに貼り開くとか、の操作方法が可能。

## Chrome に乗り換え

この度、6年ぶりくらいに普段使うブラウザをかえてみた。

Firefox + Vimperator から Chrome + Vichrome に移行しました。

[<img src="http://3.bp.blogspot.com/-znVUKjoHsnw/ToMAQ9EvSTI/AAAAAAAAAHE/2yH3d7trWhg/s1600/128.png?w=830" alt="vichrome" data-recalc-dims="1" />][2]

途中 Safari の Extensions でも Vimperatorのようなものを入れて試してみたけど、機能が少ないので Firefox に戻った経緯もあるが、今回の Chrome + Vichrome 自分が使う範囲の機能はほぼカバーしているので使い続けている。

Vichrome の設定内容はこんな感じ。

<!--more-->

<noscript>
  <pre><code class="language- ">### Hiro Fukami's Settings

# aliases
# in this example you can open extensions page by the command ':ext'
# and Chrome's option page by the command ':option'
alias ext TabOpenNew chrome://extensions/
alias option TabOpenNew chrome://settings/browser
alias downloads TabOpenNew chrome://downloads
alias history TabOpenNew chrome://history

# mappings for opening your favorite web page
nmap gf :TabOpenNew http://www.facebook.com
nmap gm :TabOpenNew https://www.facebook.com/messages
nmap gn :TabOpenNew https://www.facebook.com/notifications
nmap gt :TabOpenNew http://www.twitter.com
nmap ga :TabOpenNew http://www.amazon.co.jp
nmap gr :TabOpenNew http://www.rakuten.co.jp
#nmap gr :TabOpenNew http://www.google.com/reader
#nmap gm  :TabOpenNew https://mail.google.com/mail/#inbox

# F for continuous f-Mode
# this is recomended setting but commented out by default.
# if you want to use this setting, use the following
# nmap F :GoFMode --newtab --continuous

# you can use &lt;DISCARD&gt; to discard the key so that chrome's default
# action isn't triggered.
#nmap &lt;BS&gt; &lt;DISCARD&gt;

# if you want to change the key used to escape EmergencyMode mode,
# use emap like the following
#emap &lt;ESC&gt; :Escape

## pagecmd offers you page specific key mapping.
# in this example you can use &lt;C-l&gt;, &lt;C-h&gt; for moving between tabs
# on all web pages regardless of your ignored list setting
# because pagecmd has higher priority than ignored URLs.
pagecmd * nmap K :TabFocusNext
pagecmd * nmap J :TabFocusPrev
pagecmd * nmap d :TabCloseCurrent
pagecmd * nmap y :copyurl

# almost all Vichrome functions don't work properly for pdf contents
# so it's useful to enable default key bindings for pdf file.
pagecmd *.pdf nmap &lt;C-f&gt; &lt;NOP&gt;

# if you want to use twitter web's key binding, write settings like below
#pagecmd http*://twitter.com/* nmap f &lt;NOP&gt;
#pagecmd http*://twitter.com/* nmap r &lt;NOP&gt;</code></pre>
</noscript>

## ブックマークをEvernoteで管理へ

[<img src="http://evernote.com/media/img/products/hero_webclipper.png?w=830" alt="evernote web clipper" data-recalc-dims="1" />][3]

ブラウザを変えようと思った理由は些細なことだけど、ブックマークをはてなブックマークからEvernoteに乗り換えようと思ったことがきっかけ。

ブラウザで開いているものをEvernoteに貼りたい場合は Evernote Web Clipper が提供されているけど、Firefox Add-ons の Evernote Web Clipper はキーボードショートカットが設けられていないし、機能的にも足りない。

ブックマークしたいサイトを開いていたらキーボードショートカットだけで保存操作が完了して欲しい。  
今までは Firefox のはてなブックマーク Add-ons でキーボードだけで実現できた。  
control + shift + b で保存ウインドウが立ち上がって、タブ入力にフォーカスされているからそのままタグを指定して、Enterキーで保存。という具合。

Chrome Extension の Evernote Web Clipper にはキーボードショートカットが設けられていて、しかも保存するレイアウトに Bookmark が選択できて Evernote 上で見るのにも最適。保存先のノートブックやタグがあらかじめ指定できるのも良い。

&#8216; キーでブックマークサイドバーが出て、Enterキーを押せば保存完了。ただ、タグを追加したい時はマウスでタグ欄をクリックしないといけない。

## Vichrome 使ってみてメモ

一応、Chrome + Vichrome で使えているが、ちょっと今までに比べイマイチな点もあるのでメモっておく。

*   Vichrome のキーボードショートカットで指定したものの動作は Chrome のデフォルトより優先度が低いので、デフォルトの挙動が起きた後にアクションされるので待ちが出る 
    *   新しいタブが開いて読み込んでいる最中にタブの移動ができずに、読み込みが終わらないと操作できない
*   タブの設定が Chrome デフォルトだとできない 
    *   新しいウィンドウが開く動作でも全てタブで開くとタブを閉じたら最後に選択していたタブを表示するという設定を Firefox Tab Mix Plus で行っていた
    *   Tab Position Extensions で行えるけど、ムダな動作があるのと見ていてストレスになる
    *   一度 Chrome のデフォルト動作の最後のタブが表示されてから、動作して選択していたタブに表示が切り替わる
    *   一度 新しいウィンドウが開いてから、それが閉じられてタブで開かれる

まぁ、全ては Chrome の Extensions に対するAPIの開放具合がもたらすストレスなのだけど。  
その点 Firefox は Add-on に対してかなり深いというよりデフォルト動作を上書きできるくらい深い部分まで開放している。

完全オープンソースな Firefox と、やっぱりコントロールしたい Chrome のスタンスの違いが見れて面白い。

しばらくは Chrome で行ってみるけど、Evertnote Web Clipper が Chrome Extensions 並になったらまた戻ってしまうかも。

 [1]: https://addons.mozilla.org/en-US/firefox/addon/vimperator/
 [2]: https://chrome.google.com/webstore/detail/vichrome/gghkfhpblkcmlkmpcpgaajbbiikbhpdi?hl=en
 [3]: http://evernote.com/webclipper/