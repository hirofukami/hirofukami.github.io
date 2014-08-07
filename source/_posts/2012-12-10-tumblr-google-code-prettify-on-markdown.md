---
title: tumblrでシンタックスハイライト(google-code-prettify) on markdown
date: Mon, 10 Dec 2012 05:30:00 +0000
author: Hiro Fukami
layout: post
permalink: /2012/12/10/tumblr-google-code-prettify-on-markdown/
tumblr_hirofukami_permalink:
  - http://hirofukami.tumblr.com/post/37620313705/tumblr-google-code-prettify-on-markdown
  - http://hirofukami.tumblr.com/post/37620313705/tumblr-google-code-prettify-on-markdown
  - http://hirofukami.tumblr.com/post/37620313705/tumblr-google-code-prettify-on-markdown
tumblr_hirofukami_id:
  - 37620313705
  - 37620313705
  - 37620313705
original_post_id:
  - 39
  - 39
  - 39
categories:
  - Tech
tags:
  - google-code-prettify
  - markdown
  - Syntax coloring
  - Syntax highlighting
  - tumblr
---
tumblr にシンタックスハイライトしたコードを表示させたくて google-code-prettify を使った方法を調べたが、手順が荒かったので自分なりにまとめてみる。

<!-- more -->

# google-code-prettify のダウンロードと tumblr へのアップロード

1.  <a href="http://code.google.com/p/google-code-prettify/" target="_blank">google-code-prettify のページ</a>より、<a href="http://code.google.com/p/google-code-prettify/downloads/list" target="_blank">ダウンロードする</a></p> 
    *   フル版とsmall版があるが、フル版をダウンロードした(執筆時点のファイル名は prettify-1-Jun-2011.tar.bz2)
2.  prettify-1-Jun-2011.tar.bz2 を解凍する、prettify.css と prettify.js を確認する 
    *   google-code-prettify(解凍して作成されたフォルダ) > distrib > google-code-prettify フォルダ内にある
3.  必要があれば prettify.css を自分好みに編集する 
    *   google-code-prettify の<a href="http://google-code-prettify.googlecode.com/svn/trunk/styles/index.html" target="_blank">テーマギャラリー</a>をみて真似するとか、変えてみるとか
4.  prettify.css と prettify.js を tumblr にアップロードする 
    *   <a href="http://www.tumblr.com/themes/upload_static_file" target="_blank">tumblr のファイルアップロードページ</a>から1ファイルずつアップロードする
    *   1つのファイルのアップロードが終わったら下部に表示される URL を必ずメモっておく

# tumblr のテーマに prettify.css と prettify.js を適用

*   適用したい自分の tumblr ページから、カスタマイズ > HTMLを編集
*   </head> の直前に以下を追記する 
    *   prettify.css と prettify.js の URL パスはメモっておいたものに書き換えてください

<pre>&lt;link href="http://static.tumblr.com/c5v7n80/aq5mesksw/prettify.css" type="text/css" rel="stylesheet" /&gt;
&lt;script type="text/javascript" src="http://static.tumblr.com/c5v7n80/GOPmesh3s/prettify.js"&gt;</pre>

*   </body> の直前に以下を記述する

<pre>&lt;body onload="prettyPrint()"&gt;</pre>

*   「プレビューを更新」>「保存」を押す

# 記述する

*   ダッシュボード > テキストを押す</p> 
    *   markdown で記述したいので、それ前提で
*   載せたいコードの前に <pre><code class=&#8221;prettyprint linenums&#8221;>、後ろを </code></pre>で囲む 
    *   タグの後は改行せずにコードを載せる
*   コード内の `<` を markdown が html タグと認識してしまう場合は、`&lt;` に書き換える 
    *   公開してしまう前にプレビューして正しく表示されているか確認する必要がある
*   以下の様に記述した場合

<pre>&lt;pre&gt;&lt;code class="prettyprint linenums"&gt;class Voila {
public:
  // Voila
  static const string VOILA = "Voila";

  // will not interfere with embedded tags.
}
&lt;/code&gt;&lt;/pre&gt;
</pre>

*   以下の様な表示になる

<pre><code class="prettyprint linenums">class Voila {
public:
  // Voila
  static const string VOILA = "Voila";

  // will not interfere with embedded tags.
}
</code></pre>

*   行数表示をしたくなければ、

<pre>&lt;pre&gt;&lt;code class="prettyprint"&gt;</pre>

と、linenums を抜くと以下のようになる

<pre><code class="prettyprint">class Voila {
public:
  // Voila
  static const string VOILA = "Voila";

  // will not interfere with embedded tags.
}
</code></pre>

うまく表示されましたと。めでたしめでたし。

## 参考URL

### google-code-prettify

*   <a href="http://code.google.com/p/google-code-prettify/" target="_blank">http://code.google.com/p/google-code-prettify/</a>
*   <a href="http://tumblring.net/how-to-show-code-in-a-tumblr-post/" target="_blank">http://tumblring.net/how-to-show-code-in-a-tumblr-post/</a>
*   <a href="http://chips-tips.tumblr.com/post/6578424612/google-code-prettify" target="_blank">http://chips-tips.tumblr.com/post/6578424612/google-code-prettify</a>
*   <a href="http://sunny4381.tumblr.com/post/21555153955/using-google-code-prettify-as-syntax-highlighter-in" target="_blank">http://sunny4381.tumblr.com/post/21555153955/using-google-code-prettify-as-syntax-highlighter-in</a>

### Markdown

*   <a href="http://daringfireball.net/projects/markdown/syntax" target="_blank">http://daringfireball.net/projects/markdown/syntax</a>
*   <a href="http://blog.2310.net/archives/6" target="_blank">http://blog.2310.net/archives/6</a>

## 参考 &#8211; 自分の prettify.css

<pre><code class="prettyprint linenums">.pln{color:#000}

@media screen{
    .str{color:#080}
    .kwd{color:#008}
    .com{color:#800}
    .typ{color:#606}
    .lit{color:#066}
    .pun,.opn,.clo{color:#660}
    .tag{color:#008}
    .atn{color:#606}
    .atv{color:#080}
    .dec,.var{color:#606}
    .fun{color:red}
}

@media print,projection{
    .str{color:#060}
    .kwd{color:#006;font-weight:bold}
    .com{color:#600;font-style:italic}
    .typ{color:#404;font-weight:bold}
    .lit{color:#044}
    .pun,.opn,.clo{color:#440}
    .tag{color:#006;font-weight:bold}
    .atn{color:#404}
    .atv{color:#060}
}

pre.prettyprint{padding:2px; border:1px solid #888; overflow: auto}

ol.linenums{margin-top:0;margin-bottom:0}

li.L0, li.L1, li.L2, li.L3, li.L4, li.L5, li.L6, li.L7, li.L8, li.L9
{
    color: #555;
    list-style-type: decimal;
}
li.L1,li.L3,li.L5,li.L7,li.L9{background:#fff}
</code></pre>