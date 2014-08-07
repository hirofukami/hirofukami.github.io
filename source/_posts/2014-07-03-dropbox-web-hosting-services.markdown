---
layout: post
title: "Dropbox Webホスティングサービス一覧"
date: 2014-07-03 17:30:00 +0900
comments: true
categories: Services
---

![Dropbox](/images/dropbox-logos.png)

2014年2月に私の会社、[シェイクソウル](http://www.shakesoul.net/) で [SmartDesigning][] をリリースしました。

このサービスは、 [Dropbox](https://www.dropbox.com/) を使ってローカルPCのファイル更新をリアルタイムにWebサーバに反映するサービスで、従来のWebホスティングに比べてファイルアップロードの手間なくWebサイトを作ることができます。

カテゴライズすれば __Dropbox Webホスティングサービス__ というのでしょう。日本ではあまり見かけないのでおそらく [SmartDesigning][] くらいですが、海外はいくつかあります。

競合調査も兼ねて世界のDropbox Webホスティングサービスを調べたことをまとめます。

<!-- more -->

# Brace

![Brace](http://s3.amazonaws.com/crunchbase_prod_assets/assets/images/resized/0037/1139/371139v1-max-250x250.png)

* URL : http://brace.io/
* Twitter : https://twitter.com/braceio
* Blog : http://blog.brace.io/
* CrunchBase : http://www.crunchbase.com/company/brace

多分 Dropbox Webホスティング系のサービスで唯一TechCrunch に載ったサービス。
http://techcrunch.com/2013/12/05/brace-launches-dropbox-powered-hosting-solution-for-static-sites/

Dropbox内にフォルダが作られスタティックファイルを置くとWebサイトに反映。
Draft用URLが別にあって本番へのship(deploy)ができる。

* Pricing
   * FREE あり
   * $50/year Umlimited visitors、カスタムドメイン
   * 上位プラン Amazon's CDN、プライベート設定

2013年ローンチ
Sanfrancisco, CA, USA

# site44

![site44](http://www.site44.com/static/site44logo.png)

* URL : http://www.site44.com/
* Twitter : https://twitter.com/site44
* Blog : http://blog.site44.com/

Dropbox内にフォルダが作られスタティックファイルを置くとWebサイトに反映。
日本では [Gigazine][1], [Lifehacker][2], [NAVERまとめ][3]やいくつかのブログ載る。

[1]: http://gigazine.net/news/20130125-site44/
[2]: http://matome.naver.jp/odai/2135916317055633301
[3]: http://www.lifehacker.jp/2012/10/121005site44dropbox.html

* Pricing
   * FREE は以前あったが今はなくしている
   * $4.95/month サイト数10、データ転送量10GB/monthまで
   * 上位プランもデータ転送量リミットあり

PLANET RATIONAL
http://www.planetrational.com/
2名のアメリカ会社、1名はDropboxで働いている

# PANCAKE

* URL : https://pancake.io/
* Twitter : https://twitter.com/pancakeio
* Blog : http://blog.pancake.io/

git push(beta) と Dropbox 両方に対応している。
Markdownで書いたファイルも変換してくれる。Octopressのようなテンプレートの記述方法ができる。

プライシングの表示なし

2011年から？
運営会社不明

# KISSr

* URL : http://www.kissr.com/

Comming Soon でまだ準備段階だったが、オープンソース化した。

* Pricing
    * $5/month プランのみ、詳細わからず

個人
ライセンス表記なし

# DropPages.com

* URL : http://droppages.com/
* Twitter : https://twitter.com/droppages

テキストファイルやテンプレートファイルからなるオリジナルのテーマをDropboxに置いて、Webサイトにするサービス。オリジナルのスタティックファイルをホストしてはいけないみたい。

プライシングの表示なし

2011年5月から行われているサイドプロジェクト
個人？
イギリス サウサンプトン

# cloudcannon

* http://cloudcannon.com/
* Twitter : https://twitter.com/cloudcannonapp
* Facebook : https://www.facebook.com/CloudCannon
* Blog : http://cloudcannon.com/blog

HTML内に独自のクラスを書くとブラウザ上から編集ができる。

* Pricing
    * $9/month オリジナルドメイン、1サイト追加ごと+$5/month

ニュージーランドの会社、2013年ごろから3名でやっているようだ

# Harp Platfrom

* https://www.harp.io/

オリジナルの404ページ、オリジナルSSLに対応、オープンソース

* Pricing
    * $9.95/month カスタムドメイン、ベーシックSSL、ストレージ無制限

2012年から
カナダ バンクーバー

# そして Smart Designing
![Smart Designing](images/2013/03/slide-sd.png)

* URL : http://smartdesigning.me/
* Twitter : http://smartdesigning.me/
* Facebook : https://twitter.com/smart_designing

Dropbox内にフォルダが作られスタティックファイルを置くとWebサイトに反映。というのは同じ。
Static版に加え、**WordPress版**もリリース。日本、アジアを中心に攻める予定。

* Pricing http://smartdesigning.me/pricing/
  * ￥380/月から 1site、Storage 200MB、アクセス無制限
  * 上位プラン プライベートアクセス、オリジナルドメイン、Dropboxチーム共有

2014.02 ローンチ
Tokyo, Japan
株式会社シェイクソウル



# まとめ

どこもサービス内容の基本はDropboxを使ってWebホスティングするというもの。Webダッシュボードを設けたり、ドラフトURLを設けたりと細かなところでは違いはある。

FREEもありつつ価格は$5から$9/monthが多め。意外と[SmartDesigning][]の￥380/月からは安い方だった。

Static以外の **WordPress版** を提供していたのは [SmartDesigning][] のみだった。

まだまだどこも小ぶりな会社で、どこがこのジャンルを制するかはまだわからない。そもそもこのジャンルがブレイクするかはこれからだろう。

シェイクソウルとしては時代の先端的な部分を先駆けてやっているので、日本でこのポジションを得られているのは、まずOKと思っている。

Dropbox Webホスティング面白そうだなと思ったら、日本語サポートができる [SmartDesigning][] を是非申し込んでください。

http://smartdesigning.me/

[SmartDesigning]: http://smartdesigning.me/