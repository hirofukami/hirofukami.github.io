---
layout: post
title: "Amazon で Kindle とペーパーバックを作るやり方"
date: 2023-03-07 10:15:00 +0900
comments: true
categories:
  - Book
tags:
  - Amazon
  - Kindle
  - ペーパーバック
  - 作り方
  - Pages
description: 誰でもAmazonで本を出せるようになった。実際「ひとりスタートアップ」を出したので、そのやり方を残しておく。
image_out: https://lh3.googleusercontent.com/pw/AMWts8BEnG3yYXHJ70Ne3p6DluF4OIQblFlDo092_3l-pKB0-o-98yvaPVLfb7W6AygvK3GW8BZconYh_p_SUFvfGjvQQQsdXj_P0pNW66ONVrtwuIUsVsVx1gAI198xhg4iQKmnq4-xTdBc7XKtMkOytojOzg=w1200-h900-no?authuser=0
---
![Amazon ペーパーバック](https://lh3.googleusercontent.com/pw/AMWts8BEnG3yYXHJ70Ne3p6DluF4OIQblFlDo092_3l-pKB0-o-98yvaPVLfb7W6AygvK3GW8BZconYh_p_SUFvfGjvQQQsdXj_P0pNW66ONVrtwuIUsVsVx1gAI198xhg4iQKmnq4-xTdBc7XKtMkOytojOzg=w1200-h900-no?authuser=0)

[ひとりスタートアップ][1]で Kindle とペーパーバックを作った。

ちょっと手順や工夫が必要だったので、今後のためにメモっておく。

# Kindle の作り方

基本的には Pages で書いて、epub にしてアップロードするだけ。

## 流れ

1. Pages で書く
1. Pages で epub 形式でエクスポート
1. 表紙をデザインして png or jpeg 形式で保存
1. kdp にアップロード

## epub について

Pages から epub にした時に、

維持されるもの

* 目次
* 改ページ
* 図表の位置
* 中央、左、右揃い
* 文字の大きさ

維持されないもの

* フォントの種類
* ページ数

## Pages

### 図表の挿入

図表を挿入する際は、文字と一緒に移動してほしいので、
「Arrange 配置」にて「Move with Text テキストと移動」タブ、「Inline with Text インライン(テキストあり)」にする。

その際、テキストと同様「中央揃え」にする。

「自動」にしてしまうと、収まるスペースに図表を移動してしまう時がある。

### 目次の挿入

ドキュメントの目次を挿入する。

目次に表示する項目は、右フォーマットパネルにて指定できる。

## 表紙の作成

画像サイズは、1600px x 2560px にする。

[Canva][canva] を使った。

# ペーパーバックの作り方

## 流れ

1. Pages で書く
1. Pages で pdf 形式でエクスポート
1. 表紙のテンプレートをダウンロード
1. テンプレートを元に表紙を作成
1. 表紙と原稿を kdp にアップロード

## Pages

PDF 通りに印刷される。

余白のとり方、読みやすいフォントなど kindle 作りには必要なかった要素が大事になってくる。

### マージン設定

原稿の PDF ファイル通りに製本されるので、本の背側(閉じられる方)の余白は多めに、小口側の余白は少なめにする。

右ドキュメントパネルにて、「Document Margins」の「Facing Pages」のチェックボックスを付けると、左右のマージン設定が、Inside/Outside になる。

[ひとりスタートアップ][1]で設定したマージンは以下。

![Pages margin](/images/2023/03/20230307-screenshot_pages_margin.png)

実際に印刷されたものは、マージン + 1mm のサイズだった。

## 奇数偶数で異なるヘッダー、フッター設定

ページ数の位置はページの外側にしたかったので、奇数ページと偶数ページで異なるヘッダー・フッターの設定にしたかった。

右ドキュメントパネルにて、「Section」タブの「Headers & Footers」の「Left and right pages are different」のチェックボックスを付ける。

[ひとりスタートアップ][1]で設定したマージンは以下。

![Pages margin](/images/2023/03/20230307-screenshot_header_footer.png)

## 表紙作り

表紙は、表紙、背、裏表紙を1つの PDF ファイルで指定する。

ページ数によって、このファイルの縦横のサイズが変わるので、Amazon の表紙作成ツールでテンプレートを生成する必要がある。

1. [ペーパーバックの表紙の作成](https://kdp.amazon.co.jp/ja_JP/help/topic/G201953020)を開く
1. [表紙計算ツールとテンプレート生成ツール](https://kdp.amazon.co.jp/cover-calculator)を開く
1. パラメーターを入力する (原稿のページ数の指定が必要なので、表紙作成の前に原稿作成が終わっていること)
1. サイズの計算 ボタンを押す
1. 表示された各サイズの「全表紙」の幅と高さをメモっておく
1. テンプレートをダウンロード ボタンを押す
1. ダウンロードしたファルを解凍する

実際の表紙ファイルのデザインは [Canva][canva] を使った。

1. Canva にログイン
1. 新規作成する、キャンバスのサイズはメモっていた全表紙の幅、高さを入力する
1. テンプレートの png ファイルを追加して、キャンバスめいいっぱいまで広げる
1. テンプレートのガイドに従ってデザインする
1. 表表紙をデザインする (kindle 表紙で作った画像をそのまま挿入)
1. 背表紙に本タイトルと筆者名を入力する
1. 裏表紙に ISBN 番号を入力する
1. 出来上がったら、PDF ファイル形式でエクスポート、ダウンロードする
1. kdp の表紙としてアップロードする

## 校正用の印刷

「校正刷りを依頼」で世に出す前にチェックしたほうが良い。

余白のとり方、読みやすさなどをチェックした。

1冊 115ページで 757円ほどかかった。

[1]: https://amzn.to/3P1Szqj
[canva]: https://www.canva.com
