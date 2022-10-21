---
layout: post
title: "WordPressからOctopressへきれいに移行する方法"
date: 2014-12-01 20:00:00 +0900
comments: true
categories: Tool
---
![octopress](/images/20141201-octopress.png)

自分のブログ [hirofukami.com][] は今、Octopress on [Github Pages][] で動いています。

その前は自分サーバ上で WordPress を運用していたのですが、アップデートがめんどい、もうちょっとすっきり運用したい、あとOctopress結構クールじゃん(これが大きい)とか思って [Octopress][] に移行しました。

今までブログは以下のように渡り歩いてきたのですが、今までのポストは全て移行して蓄積してきました。

1. はてなダイアリー
2. Tumblr
3. WordPress.com
4. WordPress on 自分サーバ
5. Octopress on Github Pages

2002年12月が最初のポストで、12年間で738ポストあります。
今回の移行も今までどおり全ポストを WordPress から Octopress に移行するためにあれこれやってみたのですが、Webに載っている方法でベストなものがなかったので、少しだけいじってきれいに移行できたというお話です。

<!-- more -->

## Webに載っている方法たち

Octopress の元になっている [Jekyll][] には[WordPressからの移行方法も紹介しているページ][1]があったり、検索すると出てきたりして3つほど試してみた。

###  1. WordPress to Jekyll Exporter

Github から取得可能 https://github.com/benbalter/wordpress-to-jekyll-exporter

さすがに wordpress.org のプラグインには登録されていない。

WordPress プラグインとしてインストールして、WordPress管理画面上で操作する。
全てのポストを `.md` ファイルに変換してくれる。

動作は問題ないのだけど、生成された `.md` ファイルを見ると、最初の行のyaml front matterが以下のようになっている。

```
---
title: 仕事で本当に大切にしたいこと
author: Hiro Fukami
layout: post
permalink: /2004/07/26/post/
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

日付(date)がない。。。

試しに `rake generate` からの `rake preview` で見てみると、確かに日付が表示されない。日付がわからないのはブログとしてダメだ。何とかせねば。

### 2. jekyll-import

[Jekyllのページに紹介されていて][1] ruby の gem で提供されれている。

ソース : https://github.com/jekyll/jekyll-import/tree/v0.4.1

WordPress の動いているサーバ上で実行させるようなのだけど、うまく動かなかった。


### 3. Exitwp

もっと他にないのかと思って調べて見つけたもの。

ソース : https://github.com/thomasf/exitwp

WordPress管理画面からexportツールを使ってxmlファイルで出力させて、それを python スクリプトで変換してくれるもの。

XMLファイルを落として、手元のMacで実行してみたが色々足りないよエラーが出て、あまりMacにインストールしたくない派の自分としてはここでストップした。

## 結局、WordPress to Jekyll Exporter に1行足すだけで解決した

WordPress to Jekyll Exporter のソースを見てみると、jekyll-export.php の中で yaml front matterの部分をWordPress関数から持ってきている部分があり、そこに WordPress 関数の `get_the_time` を足したらうまく日付が加わった。

```
---
title: 仕事で本当に大切にしたいこと
date: Mon, 26 Jul 2004 15:00:00 +0000
author: Hiro Fukami
layout: post
permalink: /2004/07/26/post/
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

preview もOK。日付のフォーマットは調べたらOctopress側でも変更する必要なく解釈できるようだったので、シンプルな1行になった。

`'date' => get_the_time( 'r', $post ),`

差分は[こちら](https://github.com/d-sea/wordpress-to-jekyll-exporter/commit/50b7a10276dd8f799558502e8f4a0b7462968188)

[Github で fork している](https://github.com/d-sea/wordpress-to-jekyll-exporter)ので、使い方はご自由にどうぞ。

OK、OK。思っていた通りに移行できた。すっきりした。

良き Octopress Blog ライフを〜


## 追記

日本語エンコードされた文字の入ったパーマリンクだと、個別ページが真っ白になって見れなくなる事象がでたため、それを [解消したポストを書きました][afterpost]。

**[WordPress to Octopress 日本語パーマリンクを直す][afterpost]**

良かったら読んでやってください。

 [afterpost]: /2015/02/15/permalink-wordpress-to-octopress/

 [hirofukami.com]: http://hirofukami.com
 [Octopress]: http://octopress.org/
 [Github Pages]: https://pages.github.com/
 [Jekyll]: http://import.jekyllrb.com/
 [1]: http://import.jekyllrb.com/docs/wordpress/
