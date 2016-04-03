---
title: wp-markdown + TextMateでブログ書くのが快適になった
date: Fri, 10 Jan 2014 16:57:29 +0000
author: Hiro Fukami
layout: post
permalink: /2014/01/10/wp-markdown-textmate/
categories:
  - Life
tags:
  - blog
  - markdown
  - TextMate
  - tips
  - wordpress
  - wp-markdown
---
## Markdownで書けばいいじゃん

<img src="https://raw2.github.com/dcurtis/markdown-mark/master/png/208x128.png" width="208" height="128" class="alignnone" />

WordPress はCMSとしてはダントツで使えるツールなのだけど、ブログを書く時にWebインターフェイス上からだと、HTMLベタ書きかアイコンをクリックしてとかになって、どうも書く気が失せてしまっていた。

そんな時に Markdown で寄稿いただいたことをきっかけに、「そうか Markdown で書ければ良いじゃん」と気づいた。

結構チームでプロジェクトに取り組んでいた時は PukiWiki やら Redmine やらでドキュメントをガシガシ書いていたので、その書きやすさは実感済みだったのに、なんで今まで思いつかなかったのだろう。

## MarkDownで書ける wp-markdown プラグイン

プラグインを探してみたらやっぱりあった。 プラグインは [wp-markdown][1]。

これを入れて有効化して、「設定」-「投稿設定」の下部にある「MarkDown」で設定する。 今回は投稿する時だけ Markdown で書きたいので、ヘルプバーやシンタックスハイライトもなしでシンプルに。

<!--more-->

[<img src="/images/2014/01/Screen-Shot-2014-01-10-at-15.53.13.png?resize=422%2C343" alt="Screen Shot 2014-01-10 at 15.53.13" style="border: 5px solid #dcdcdc;" class="alignnone size-full wp-image-2171" data-recalc-dims="1" />][2]

注意点は今まで書いた記事を編集する時に Markdown 的に書き換えてしまうので、他のプラグインとの競合でレイアウトが崩れてしまったりHTMLタグがそのまま表示されてしまったりする。 自分の場合はシンタックスハイライターの SyntaxHighlighter Evolved で書いたコードの改行が削除されてしまいました。

編集することはあまりないし、今後コードを載せたい時は Github Gist から貼る方法にするので、まぁ良いかなとということにしています。

## 書くのは TextMate

このままの状態でWebインターフェイス上の新規投稿を追加に Markdown 形式で書けるけど、より快適さを求めてエディターとして使っている [TextMate][3] で書いてからそれをコピペするようにしている。

TextMate はテキストエディターで豊富なショートカットや充実した補完機能を持っていて、いまだに使い倒せないくらい機能が多い。 Markdown にも対応していて綺麗にハイライトしてくれたり、ある程度プレビューしてくれたりする。

ちなみにこの記事をTextMateで書いているスクリーンショットはこんな感じです。

[<img src="/images/2014/01/Screen-Shot-2014-01-10-at-16.25.29.png?resize=394%2C449" alt="Screen Shot 2014-01-10 at 16.25.29" style="border: 5px solid #dcdcdc;" class="alignnone size-full wp-image-2172" data-recalc-dims="1" />][4]

Macユーザは多いと思うけれど、自分は Caps lock と Control キーを入れ替えて、

*   Control + a で行頭にカーソル移動
*   Control + b で一文字戻る
*   Control + k でカーソルから文末まで削除してメモリに入れてからの
*   Control + y でヤンク

みたいにMacのキーボードショートカットを多用している。十字キーは全く使わず、指のポジションも固定されるので早くなる。 参考 : [OS X のキーボードショートカット][5]

TextMateでもこれらのショートカットが使えて、もしショートカットが競合してしまう場合は変更もできる。

ということでストレスなくブログが書けるようになりました。お気に入りのテキストエディターで書けるのが良いなぁ。年始からガシガシ書けています。

良かったら参考にしてみてください。 みなさんはどんなふうに記事を書かれていますか？

 [1]: http://wordpress.org/plugins/wp-markdown/
 [2]: /images/2014/01/Screen-Shot-2014-01-10-at-15.53.13.png
 [3]: http://macromates.com/
 [4]: /images/2014/01/Screen-Shot-2014-01-10-at-16.25.29.png
 [5]: http://support.apple.com/kb/ht1343?viewlocale=ja_JP