---
title: iAd と AdMob の実測比較結果
date: Tue, 19 Mar 2013 11:24:17 +0000
author: Hiro Fukami
layout: post
permalink: /2013/03/19/iad-vs-admob/
categories:
  - Tech
tags:
  - AD
  - AdMob
  - apps
  - comparison
  - develop
  - iAd
  - report
  - test
---
今年のテーマは[「トライ＆エラー」と宣言していて][1]、その一つとしてスマートフォンアプリでビジネスを成り立たせるための実験をしている。

最近は「無料+AD表示」の傾向が多い気がするけど、本当にこれだけで儲かるのか気になったので試してみることにした。

アプリの上とか下とかにアプリと関係ないバナーを表示すること自体、本当にその形態がユーザとアプリ提供者にとって良いことなのかはかなり疑問だが、まずは実験、やってみた。

試したアプリは2つ。

<a href="https://itunes.apple.com/jp/app/dm-chat/id447521974?mt=8" target="_blank"><img class="alignnone size-full wp-image-772" alt="dmatchat_128x128" src="/images/2013/03/dmatchat_128x128.png?resize=128%2C128" data-recalc-dims="1" /></a> TwitterのメンションとDMだけをチャットみたいにやり取りできる、DmAtChat。

<a href="https://itunes.apple.com/jp/app/ftupdater/id448808656?mt=8" target="_blank"><img class="alignnone size-full wp-image-784" alt="ftupdater_icon_128x128" src="/images/2013/03/ftupdater_icon_128x128.png?resize=128%2C128" data-recalc-dims="1" /></a> FacebookとTwitterにマルチポストするだけのアップデータ、FTupdater。

この2つは Titanium でアプリ開発を学ぶために作って、すでにリリースしてから1年以上経過してダウンロード数は少ないけど、割と色々な国で使われている。

無料で出していたのでこれに iAd と Admob を表示する。割合としては iAd の表示がエラーになった場合に Admob を表示するようにする。

ADを仕込んでから1ヶ月ほど経過したので、結果を出してみる。

<!--more-->

# 結果

## Fill Rate 比較

iAd で期間は2週間。上が DmAtChat, 下が FTupdater

[<img class="alignnone size-full wp-image-778" alt="Screen Shot 2013-03-19 at 7.21.56" src="/images/2013/03/screen-shot-2013-03-19-at-7-21-56.png?resize=474%2C125" data-recalc-dims="1" />][2]

iAd の Fill Rate が55％以下くらい。

iAd Fill Rate は 80% 前後で100％にいかない、iAd Fill Rate は多分 iAd が利用できる環境でも広告の在庫がなくて表示できていないということなのだろう。

実質の Fill Rate は

> 53.62% x 78.41% = 42.04%
> 
> 54.77% x 81.28% = 44.52%

とそれぞれなって、50%を切ってしまう。機械損失が50%以上というのは非効率すぎるしビジネス的にダメと判断できる。

なぜこんなに低いのかというと、

[<img class="alignnone size-medium wp-image-779" alt="Screen Shot 2013-03-19 at 7.28.53" src="/images/2013/03/screen-shot-2013-03-19-at-7-28-53.png?resize=300%2C218" data-recalc-dims="1" />][3]

[<img class="alignnone size-medium wp-image-781" alt="Screen Shot 2013-03-19 at 7.37.20" src="/images/2013/03/screen-shot-2013-03-19-at-7-37-20.png?resize=300%2C206" data-recalc-dims="1" />][4]

Fill Rate 0% の国が多いので平均を下げてしまっている。0%というのは iAd 未提供エリアということなのだろう。

提供できている国はアメリカ、日本、オーストラリア、カナダ、ヨーロッパの一部と先進国の一部に限られる。提供国はだいたい Fill Rate 100% だが、60%のカナダ、ついでアメリカが82%で広告在庫を貯めれていないことがわかる。アップルお膝元アメリカが他国に比べて低いのがなかなか面白い。

一方、Admob、期間は約1ヶ月。同じく上が DmAtChat、下が Ftupdater

[<img class="alignnone size-full wp-image-787" alt="Screen Shot 2013-03-19 at 7.27.46" src="/images/2013/03/screen-shot-2013-03-19-at-7-27-46.png?resize=791%2C72" data-recalc-dims="1" />][5]

[<img class="alignnone size-full wp-image-788" alt="Screen Shot 2013-03-19 at 7.28.23" src="/images/2013/03/screen-shot-2013-03-19-at-7-28-23.png?resize=784%2C65" data-recalc-dims="1" />][6]

広告表示率(Fill Rate)はほぼ100％。さすが Adsense・AdWordsを提供してきた歴史もあり全世界でよどみなく広告配信できている。

Apple と Google は両方共グローバルカンパニーとして全世界で展開しているイメージになりそうだが、ADネットワークに関しては、まさにグローバルカンパニーだった Google と後発でまだまだ未熟だった Apple という結論になった。

## CPM / CTR 比較

### iAd

<table border="1">
  <tr>
    <th>
      DmAtChat
    </th>
    
    <th>
      FTupdater
    </th>
  </tr>
  
  <tr>
    <td>
      CPM $0.35
    </td>
    
    <td>
      CPM $1.15
    </td>
  </tr>
  
  <tr>
    <td>
      CTR 0.13%
    </td>
    
    <td>
      CTR 0.56%
    </td>
  </tr>
</table>

### AdMob

<table border="1">
  <tr>
    <th>
      DmAtChat
    </th>
    
    <th>
      FTupdater
    </th>
  </tr>
  
  <tr>
    <td>
      CPM $0.15
    </td>
    
    <td>
      CPM $0.57
    </td>
  </tr>
  
  <tr>
    <td>
      CTR 0.51%
    </td>
    
    <td>
      CTR 1.49%
    </td>
  </tr>
</table>

CPM の値自体はアプリの内容によって変わったりするので、同じアプリ上で iAd と AdMob を比較すると iAd の方が CPM は高い。

CTR については DmAtChat は AdMob の方が良くて、FTupdater は iAd の方が良かった。ただこれはたまたま表示された広告の内容が気になったから押しりすると思うので、これだけ見るとどちらがユーザの見たい広告だったかはわからなかった。

# まとめ

*   iAd はまだまだ提供エリア的にも広告在庫的にも未熟
*   AdMob は安定の100%広告表示率だが、CPM が低い
*   なので、「iAd でエラーになった時に AdMob を表示する」処理は高い CPM を狙いつつ機械損失をなくせるのでとても有効な方法といえる

# おまけ

「無料+AD表示で食べていけるか」という問に関しては、今回の結果より CPM は $0.15 &#8211; $1.15  だったが高めで $1.0 だとして。仮に月額100万円を目標にした場合、

*   CPM $1.0 : 100万円 / ($1.0 x ￥95/$) x 1000 = 1052万インプレッション/月

毎日使ってくれるアプリが提供できたとして、1ユーザあたり1日2回アプリ立ち上げ、1回につき10インプレッションだとすると、20インプレッション/日 x 30 = 600インプレッション/月

*   1052万インプレッション / 600インプレッション・ユーザ = 17,533 ユーザ

これくらいのユーザ数が必要になる。会社をやる場合これで一人分くらい、2人なら倍。1アプリで2万monthlyアクティブユーザ(登録ユーザ数ではない)を獲得するのは結構な成功だから、それくらいのレベルを目指さないと食べていけないよということになる。

現実的には、ユースケースの全く異なるアプリをいくつか出してトータルで1000万インプレッション/月を目指す。というところだろう。

ただこれはあくまで「無料+広告だけで」ビジネスモデルを達成するための戦術で、実際そのモデルで突き進みたいかは、個人の価値観で異なるところだろう。個人的には広告だけに頼るのではなくサービス自体の価値が対価として評価されて売上が上がるモデルの方を取りたいと思っている。

ということで、実際の iAd と AdMob の実験結果でした。

 [1]: http://hirofukami.com/2013/01/14/2013/ "2013年のテーマは「トライ＆エラー」"
 [2]: /images/2013/03/screen-shot-2013-03-19-at-7-21-56.png
 [3]: /images/2013/03/screen-shot-2013-03-19-at-7-28-53.png
 [4]: /images/2013/03/screen-shot-2013-03-19-at-7-37-20.png
 [5]: /images/2013/03/screen-shot-2013-03-19-at-7-27-46.png
 [6]: /images/2013/03/screen-shot-2013-03-19-at-7-28-23.png