---
layout: post
title: "そして Vichrome から Vimperator に戻った"
date: 2014-04-04 09:00:00 +0900
comments: true
categories: Tool
---

過去に [Firefox + Vimperator から Chrome + Vichrome に乗り換えた][oldpost] というポストを書いたのだけど、あれから2ヶ月もたたないうちに Vimperator に戻りました。

![Vimperator](http://teramako.github.io/doc/browser-workshop-20101127/vimperator-logo.png)

Chrome 短い間だったけどありがとう。Frirefox 離れてみて君の良さがわかったよ。ただいま。

理由は単純。体感的に Vichrome の不満点が大きくて、Vimperator の方がレスポンスが良く気持よく使えると感じたから。

# Vichrome の不満点

## デフォルトの処理待ちの遅さ

たとえば `d` キーを押してタブを閉じる動作をする時は、そのページの読み込みが終わっていないと動作しない。間違えて開いてしまったタブをすぐ閉じたい時もページの読み込みが終わるまで動作をしてくれないので、待ちが発生する。

タブを閉じた時の動作も同じで、最後に選択していたタブにフォーカスさせる設定も、Chrome のデフォルト動作である最終タブ(右端)にいったんフォーカスしてから、Vichrome が最後に選択していたタブにフォーカスを変えるという動作をしていることが見てわかる。

強制的に新しいウインドウではなくタブで開く動作も、一度ウインドウが開く => 閉じる => タブで新たに開く。という動作をしているのを見るはめになり非常にストレスを感じる。

これだと先回りしてキーを押す流れを描いてサクサク操作することができないので、ストレスになる。

## タブ周りの設定が少ない

`u` キーで直近で閉じたタブを開く場合、Vimperator だとそのタブの回覧履歴も残っているので、`u` 押して、`Shift + h` で一つ前に見ていたページに戻るみたいなことができるけど、Vichrome だと回覧履歴の復元まではやってくれない。

間違えて閉じてしまった時のリカバリーとしては履歴が残っていないのは痛い。

基本的にタブ周りの設定項目が少なく Firefox Tab Mix Plus のようなことができない。デザイン周りはそんなに気にしないけど動作に関わる設定が少ないのは操作性に関わるからきつい。

## 根本原因は Chrome の設計思想

Vichrome が Chrome をコントロールできる権限はデフォルト設定より低いみたいで、デフォルトの動作待ちが発生しやすいのだと思う。

`Chrome 本体 > デフォルトのセッティング > Vichrome (extensions)`

Vichrome 的にはデフォルト動作が終わってからそれを修正するというスタンスになる。

Firefox の Extensions はデフォルトの設定と同等で設定内容を書き換えることができるくらい高い権限を持っていると思う。

`Firefox 本体 > デフォルトの設定 = Vimperator (extensions)`

Chrome が extensions に対して権限を与え過ぎないという考え方からくることなので、これは Chrome の設計思想を変えない限りどうにもならないのではと思っている。

<!--more-->

# Vimperator でよくなった点

## 最初のフォームにフォーカスできるショートカットキーができた

主に Google や Amazon の検索キーワードを入れる部分に素早くフォーカスしたい時に使います。

Vichrome では `i` キーがそうだったのですが、Vimperator はなかったので、`f` でリンク先を選んで選択肢てフォーカスさせるという手順を踏む必要がありました。

いつの間にか `gi` でそのページの最初にあるフォームにフォーカスされるようになりました。

たまたま違うことを調べていた時に StackOverflow の回答に書いてあったのですが、いつから使えるようになっていたのかは知らないのですが、こういう情報はどこで仕入れればいいのだろう。

## ignorekeys が使えるようになった

Twitter, Gmail, Freedly とかサービス側のキーボードショートカットを使いたいので、今までは Vimperator プラグインの FeedSomeKeys を使っていました。

Vimperator 側から見てそのURLに対して exclude するキーをあらかじめ設定しておく方法です。
ただ、使いたいショートカットを記述する方法は大量に記述する必要があってちょっとめんどいと思っていました。

知らない間に Vimperator に ignorekeys という機能が増えていて、これで解決することができています。
これはサービス側から見て exclude する Vimperator のキーを登録するので、FeedSomeKeys とは逆の設定の仕方になります。

サービス側のキーは全部使いたくて、Vimperator のキーは必ず使いたいキーはそんなに多くないので、ignorekeys の方がマッチします。

コマンドラインモード(:)にして、

`ignorekeys add mail.\google\.com -except keys :,d,t,<C-p>,<C-n>,gi,<S-j>,<S-k>,<S-h>,<S-l>,y,f,<S-f>`

みたいに設定しています。

## Evernote Clearly でブックマーク

![Evernote Clearly](http://evernote.com/media/img/products/hero_clearly.png)

はてブに代えて Evernote にURLブックマークしているのですが、Vichrome では Evernote Web Clipper がキーマップも生かしつつショートカットキーでさくっとブックマークできて、重宝していました。

Firefoxの extensions の Evernote Web Clipper はショートカットキーが提供されていなかったのですが、Evernote から別の extension である [Clearly][clearly] が提供されていました。
これは簡易ビューをつくって Evernote に保存してくれ、ショートカットキーも用意されています。もちろん URL も含んで。

操作としては、

1. `i` キーでignoreモードにして
1. `Ctrl + Command + s` でクリップ、Evernote に保存
1. `i` キーでignoreモード終了
1. `d` キーでタブ閉じる

みたいな流れで、マウスを使うことなくキー操作だけでブックマーク出来るようになりました。

## .vimperatorrc

ということで、Firefox + Vimperator に戻りました。

ついでに、今使っている Vimperator の .vimepratorrc を gist にあげました。
プラグイン設定で FeedSomeKeys の設定が書かれていますが、ignorekeys の設定をしているので実質的に使っていません。主にキーマップだけの設定になっています。

**[GitHub Gist vimperatorrc](https://gist.github.com/d-sea/ca08b0cb765f9a255d26)**

{% gist ca08b0cb765f9a255d26 %}

[Vimperator プラグインの Github][plugins] を見ると、更新も止まっているしコミットのコメントも何ともな感じなので、Vimperator プラグインは使わないようにしています。

ちなみに本家 [Vimperator のバージョン履歴][versions] を見るに一定のペースでリリースが続けられています。

## 入れている Firefox Extensions

入れては消してを繰り返して最近は少なめにしています。

![firefox-extensions](/images/20140402-firefox-extensions.png)


## Vimperator のわからないところ

使っている歴は長いのですが、まだまだ使いこなせていないところやどうやっていいかわからない点があるので、どなたか教えていただければです。

* EMBED モードからキーを押して抜けたい
  * 現状はどこか支障のなさそうな部分をクリックして抜けている


どなたかの何らかのお役に立てればです。では、快適なブラウジングライフを〜

[oldpost]:http://hirofukami.com/2014/02/04/firefox-vimperator-vs-chrome-vichrome/
[plugins]: https://github.com/vimpr/vimperator-plugins
[versions]: https://addons.mozilla.org/ja/firefox/addon/vimperator/versions/
[clearly]: https://addons.mozilla.org/en-US/firefox/addon/clearly/