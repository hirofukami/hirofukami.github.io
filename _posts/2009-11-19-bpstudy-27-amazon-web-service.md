---
title: 'BPstudy#27 で「Amazon Web Service にまつわるいろいろ」を話してきました'
date: Thu, 19 Nov 2009 15:00:00 +0000
author: Hiro Fukami
layout: post
permalink: /2009/11/19/bpstudy-27-amazon-web-service/
tumblr_hirofukami_permalink:
  - http://hirofukami.tumblr.com/post/29678348344/bpstudy-27-amazon-web-service
  - http://hirofukami.tumblr.com/post/29678348344/bpstudy-27-amazon-web-service
  - http://hirofukami.tumblr.com/post/29678348344/bpstudy-27-amazon-web-service
tumblr_hirofukami_id:
  - 29678348344
  - 29678348344
  - 29678348344
original_post_id:
  - 207
  - 207
  - 207
categories:
  - Tech
tags:
  - AWS
  - BPStudy
  - event
  - report
---
<div class="section">
  <p>
    今まで何度か参加している BPstudy で Amazon EC2 について話してくれませんか。というリクエストを受けて、<a href="http://beproud.jp/bpstudy/?p=37" target="_blank">BPstudy#27</a> で話してきた。
  </p>
  
  <p>
    持ち時間は90分で大学の講義並みのたっぷりな時間だった訳ですが、なんとかおさめました。
  </p>
  
  <p>
    話す範囲はあまり隠すのも変だし、部分的にとどめるのもいやらしいので、素直に自分が今まで AWS を触ってきて知っていることをいろいろ出していこうと思い、対象は広範囲に、でも実践的に動く様子を示すことにしました。
  </p>
  
  <p>
    機能説明の後には必ずデモを行い、時間稼ぎ＆実際を見せるようにしました。デモがこけるとすると時間が足りずに最後まできれいに行かない懸念もありましたが、とりあえず全てのデモがつまるところなく見せれたのは個人的に満足でした。
  </p>
  
  <p>
    当日の様子は <a href="http://twitter.com/#search?q=%23bpstudy" target="_blank">twitter の #bpstudy タグ</a> で見れます。
  </p>
  
  <p>
    その時のスライドは以下。
  </p>
  
  <p>
  </p>
  
  <div style="margin-bottom: 5px;">
    <strong> <a title="2009.11.20 BPstudy#27 Amazon Web Service" href="https://www.slideshare.net/d_sea/20091120-bpstudy27-amazon-web-service" target="_blank">2009.11.20 BPstudy#27 Amazon Web Service</a> </strong> from <strong><a href="http://www.slideshare.net/d_sea" target="_blank">Hiro Fukami</a></strong>
  </div>
  
  <p>
    会場の質問内容や様子を見る限り、EC2 でインスタンス起動はさせてみたけどもそこまでの方が多かったのかと思えた。今回、Elastic Load Balancing, Cloud Watch, Auto Scaling など、EC2 の新機能も実践的に紹介出来たので AWS のサービスの幅広さから可能性の大きさを感じて、おもしろがってくれればいいなと思います。
  </p>
  
  <p>
    実は、今回のスライドを準備する途中で新たに発見したことが2つあった。
  </p>
  
  <ul>
    <li>
      Auto Scaling に —load-balancers のオプションを付けるとスケールアウトさせたインスタインスを自動的に Elastic Load Balancing の分散対象に追加してくれる
    </li>
    <li>
      Elastic Load Balancing の分散対象のインスタンスへのヘルスチェックは、転送先となるポート番号が空いているかどうか。が、デフォルトの内容になっているようだ
    </li>
  </ul>
  
  <p>
    今まで不明な点だったので、これは見つけられてよかった。
  </p>
  
  <p>
    自分のレベルを一つ上げるのは外的要因の方が起こりやすい。というのを感じてしまった。本当は自分自身の意志で行ければいいけど、甘えが出るからな。
  </p>
  
  <p>
    今後も BPstudy は時間があえば行きたい勉強会なので、技術的に濃い刺激を受けたい人は是非 BPsudy へどうぞ。お会いしましょう～
  </p>
</div>