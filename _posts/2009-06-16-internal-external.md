---
title: ネットに関わっていると言っても internal/external で全く違うと意識した方がいい
date: Tue, 16 Jun 2009 15:00:00 +0000
author: Hiro Fukami
layout: post
permalink: /2009/06/16/internal-external/
tumblr_hirofukami_permalink:
  - http://hirofukami.tumblr.com/post/29678337022/internal-external
  - http://hirofukami.tumblr.com/post/29678337022/internal-external
  - http://hirofukami.tumblr.com/post/29678337022/internal-external
tumblr_hirofukami_id:
  - 29678337022
  - 29678337022
  - 29678337022
original_post_id:
  - 232
  - 232
  - 232
categories:
  - Self Management
tags:
  - Business
  - Internet
---
<div class="section">
  <p>
    あるサーバメーカの方に聞いたお話の内容が、結構現状のネットを中心としたビジネスの特徴を明確化している気がしたのでまとめてみる。
  </p>
  
  <p>
    そのメーカではインターネットを使ったシステムは internal と external の2つの特徴に分けられて、その2つは全く考え方とアプローチが異なるためにそれぞれに対してマッチするサーバを開発しているということだった。
  </p>
  
  <p>
    それぞれの特徴をまとめると、
  </p>
  
  <ul>
    <li>
      internal とは</p> <ul>
        <li>
          ITを業務で利用する企業内IT、業務ASP的なシステム
        </li>
        <li>
          利用者はその会社の社員でシステム責任者は情シス担当者
        </li>
        <li>
          SIer にお金を払って作らせるケースが多い
        </li>
        <li>
          信頼性やセキュリティの意識が強くそのためのコストはいたし方ないという考え方
        </li>
      </ul>
    </li>
    
    <li>
      external とは <ul>
        <li>
          みずからサービスなりを提供している形態、サービスプロバイダやインターネットサービス運営会社
        </li>
        <li>
          利用者は不特定多数のコンシューマで会社としてサービス提供している
        </li>
        <li>
          自分たちで開発、自分たちで作っちゃうスタンス
        </li>
        <li>
          なるべくベンダ異存のないプラットフォームを好む
        </li>
        <li>
          なるべくお金はかけたくないしそもそも最初はお金持ってない
        </li>
      </ul>
    </li>
  </ul>
  
  <p>
    というお話で、external 側の消費電源やスペースへの要求レベルが高く、今までちゃんと提供できていなかったので、そこをターゲットとしたサーバを作ってますとのことだった。
  </p>
  
  <p>
    自分としては internal は社内で使うシステムのサーバを IDC に置いて、firewall をきつめに設定して社内からのみアクセスできるネットワークを作って業務中はアクセスあるけど、お休みの日は使われないような運用をイメージした。一方、external ははてな、Yahoo!やGoogleなどいわゆるコンシューマを相手に24時間ネットサービスを提供し続けているところとイメージした。
  </p>
  
  <p>
    自分の中では何気なく分けていたのだけどそれを明確化していて、しかもすでに戦略を打っているところはさすがメーカ、手堅いな。と思ったところだった。
  </p>
  
  <p>
    その後、自分なりに反芻してみるとこの分け方はシステムの特徴だけにとどまらず、作ることに対する文化そのものも異なることに気づいた。結構肝なのがこの2つは同じ IT というジャンルにまとめられてしまって、働いている人たちも混同していて「ITやってます」とか言ってしまっていて、分ける意識がなく過ごしてしまっていることがあるかと思う。
  </p>
  
  <p>
    この internal/external は大事にしたいポイントなど考え方や文化からして全く異なる別ものだと思った方がいい。仕事の回し方や考え方とビジネスのスタンスの面から自分なりにまとめてみると、
  </p>
  
  <ul>
    <li>
      internal</p> <ul>
        <li>
          信頼性、確実性を重視
        </li>
        <li>
          工数主義&#160;: 数の論理で問題解決する
        </li>
        <li>
          スピード遅い
        </li>
        <li>
          スキルよりも調整能力が必要
        </li>
        <li>
          しっかりした雇用形態と緩やかな給料上昇率
        </li>
      </ul>
    </li>
    
    <li>
      external <ul>
        <li>
          スピード重視
        </li>
        <li>
          パフォーマンス主義&#160;: いいパフォーマンスする人はすごいと思われる
        </li>
        <li>
          スキルがない人はいりません
        </li>
        <li>
          小さくたって世界を変えられるぜと思える
        </li>
        <li>
          いつ辞めるか/辞めさせられるか分かりません
        </li>
      </ul>
    </li>
  </ul>
  
  <p>
    これくらい違うと思う。法人営業のSEをしていた時に internal 側にいて、サービスを立ち上げた時に external 側にいて両方を見てきた自分にとっては internal をやっている人と external をやっている人とは根本的な考え方が違うので一緒に仕事をするのは難しいというか、そもそもスタンスが違うのでそのギャップから問題が出るケースがほとんどだと思っている。振り返ってみるとベンチャーのスピードを大手のメーカや法人顧客を多く抱えるIDCに求めても実際ついて来れなかったし、その会社のシステム上そもそも無理だったりしている。
  </p>
  
  <p>
    で、自分はどちらでプレイしているのか分かっているのも結構大事で、internal な世界の中で external やってみようと言うのはそれは難しく、自宅に帰ってから個人レベルでやるか、本当に仕事としてやりたいなら external をやっているところに転職してしまうのがいいのだろう。自分のやっている仕事が実際どちらなのか分かっていれば自分のキャリアを描きやすくなるだろうし、転職するにしてもその会社がどちらかを見極められれば無駄な面接は減るだろう。
  </p>
  
  <p>
    同じように会社を作るなりして自分でビジネスを展開する時にどちら側に立つかというのも結構大事で、どちらもやるというのはやはり難しいことだと思う。
  </p>
  
  <p>
    そういう意味では、<a href="http://www.shakesoul.net/" target="_blank">ShakeSoul</a> は external で生きていく会社にしたくて、そのフィールドにどっぷり浸かるために作った訳で、最初は internal で直近の売り上げを短期的に、、、なんて考えていたことが矛盾していることに気づかされた。具体的にどういう理想を描いて追求していくかは別エントリーにまとめたいと思うが、これからも external な世界で生きていくための模索とチャレンジを続けていこうとやっぱり思うのだった。
  </p>
</div>