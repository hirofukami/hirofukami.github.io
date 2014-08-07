---
title: 'hbstudy#3 で「BGPのお話」というお話をしてきた'
date: Fri, 11 Sep 2009 15:00:00 +0000
author: Hiro Fukami
layout: post
permalink: /2009/09/11/hbstudy-3-bgp/
tumblr_hirofukami_permalink:
  - http://hirofukami.tumblr.com/post/29678343392/hbstudy-3-bgp
  - http://hirofukami.tumblr.com/post/29678343392/hbstudy-3-bgp
  - http://hirofukami.tumblr.com/post/29678343392/hbstudy-3-bgp
tumblr_hirofukami_id:
  - 29678343392
  - 29678343392
  - 29678343392
original_post_id:
  - 217
  - 217
  - 217
categories:
  - Tech
tags:
  - BGP
  - hbstudy
  - Network
  - report
---
<div class="section">
  <p>
    <a href="http://heartbeats.jp/" target="_blank">ハートビーツ</a>さん主催で行われているインフラエンジニアのための勉強会 <a href="http://heartbeats.jp/hbstudy/" target="_blank">hbstudy</a> の3回目で、BGP の話をしてきた。
  </p>
  
  <p>
    私が1つ目のネタで、2つ目のネタはTOMOYO Linuxの紹介で、NTT-DATA 沼口さんだった。
  </p>
  
  <p>
    タイトルは「BGPのお話」。思いっきりネットワークのみの世界の話なのと、BGP自身なじみがないと思ったので、どの程度伝わったのかはよく分からずとも、とりあえず概要から運用的な話までしてきた。
  </p>
  
  <p>
    勉強会中の会場の様子は <a href="http://twitter.com/#search?q=%23hbstudy" target="_blank">twitter で結構流れた</a>ので、そちらでも結構反応が分かる。
  </p>
  
  <p>
    このスライドは以下。
  </p>
  
  <div style="width:425px;text-align:left;" id="__ss_1988175">
    <a href="http://www.slideshare.net/d_sea/090910hbstudy3bgp" style="font:14px Helvetica, Arial, Sans-serif;display:block;margin:12px 0 3px;text-decoration:underline;" title="090910hbstudy#3-BGP" target="_blank">090910hbstudy#3-BGP</a> <div style="font-size:11px;font-family:tahoma, arial;height:26px;padding-top:2px;">
      View more <a href="http://www.slideshare.net/" style="text-decoration:underline;" target="_blank">presentations</a> from <a href="http://www.slideshare.net/d_sea" style="text-decoration:underline;" target="_blank">Hironobu Fukami</a>.
    </div>
  </div>
  
  <p>
    質問では経路乗っ取りについてが結構多かった。統一されたシステマチックな仕組みがあるようなイメージを持っていたようだったが、「インターネット運用は性善説に基づいている」ことがもう少し説明できれば良かったかな。
  </p>
  
  <p>
    デモでは Amazon EC2 上で instance 2つに Quagga 入れて bgpd 動かして peer 張るところまで見せたが、これだけだとルーティングしていることにはならないので、instance 7つくらいで AS path 長を持たせた構成を作ってみたら実践的になるかと思うので、やってみようと思う。
  </p>
  
  <p>
    EC2 上での 構築と動作させてみたことについては別エントリーでこのブログでまとめることにする。
  </p>
  
  <p>
    実際BGP運用をしていたのは4年以上前だから、自分の記憶が頼りだった部分もあるのだが、それなりに覚えていたし、考え方の概要は忘れていなかったことが自覚できたのは自分なりの収穫。経験したこととかを振り返ることもできたし、スライドとしてはBGPを概要からまとめられたそれなりな資料が作れたので良い蓄積ができた。
  </p>
  
  <p>
    まだまだ BGP 設計とか体制作りのコンサルティングはできる感触を得られた。
  </p>
  
  <p>
    勉強会には初参加の人たちも増えているようで、3回目にして hbstudy の認知が広まってきている感じがした。
  </p>
  
  <p>
    今後勉強会で聞いてみたいネタとしては、
  </p>
  
  <ul>
    <li>
      Apaceh にすごく詳しい人</p> <ul>
        <li>
          特に proxy とか、proxy_loadbalancer に詳しい人とか、運用ノウハウに長けている人
        </li>
      </ul>
    </li>
    
    <li>
      MySQL にすごく詳しい人 <ul>
        <li>
          tips 満載な人
        </li>
      </ul>
    </li>
    
    <li>
      モニタリングツールとか設定内容
    </li>
    <li>
      たくさんのサーバの設定内容を同期させるツールとか、tips とか
    </li>
  </ul>
  
  <p>
    かな。基本的にみんな使っているけど、あまり自信を持って動かせていないような気がするポイントかな。(自分自身がそうだったりする。。。)
  </p>
  
  <p>
    hbstudy はインフラをちゃんと学ぶ場としてはおそらく他になく貴重だから、今後も濃く良い交流が生まれる勉強会であってほしいかな。
  </p>
  
  <p>
    ということで、インフラを学びたい人は hbstudy へ一度ご参加ください。
  </p>
</div>