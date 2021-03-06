---
title: 'CLOUD EXPO &#038; RightScale User Conference in US に行ってきた'
date: Sun, 14 Nov 2010 15:00:00 +0000
author: Hiro Fukami
layout: post
permalink: /2010/11/14/cloud-expo-rightscale-user-conference-in-us/
tumblr_hirofukami_permalink:
  - http://hirofukami.tumblr.com/post/29678360078/cloud-expo-rightscale-user-conference-in-us
  - http://hirofukami.tumblr.com/post/29678360078/cloud-expo-rightscale-user-conference-in-us
  - http://hirofukami.tumblr.com/post/29678360078/cloud-expo-rightscale-user-conference-in-us
tumblr_hirofukami_id:
  - 29678360078
  - 29678360078
  - 29678360078
original_post_id:
  - 186
  - 186
  - 186
categories:
  - Business
tags:
  - Internet
---
<div class="section">
  <p>
    2週間ほど久しぶりにシリコンバレーに行ってきた訳ですが、その目的の一つが本場の<a href="http://cloudcomputingexpo2010west.sys-con.com/" target="_blank">CLOUD EXPO</a>を見ることだった。
  </p>
  
  <p>
    まだ知らないクラウドサービスがあるかどうか、またfluxflexと同じようなサービスがあるかどうか、最新の情報がそこに行けばあると思っていた。<a href="http://www.rightscale.com/lp/user-meetup.php" target="_blank">RightScale User Conference</a> に申し込むとCLOUD EXPOの参加費がタダになるということで、こちらもついでに見てみようという感じだった。
  </p>
  
  <p>
    で、結果的にどうだったかというと、CLOUD EXPOは非常に残念な場だった。むしろRightScaleの話のほうが実践的で内容は良かった。
  </p>
  
  <p>
    <a href="http://f.hatena.ne.jp/d_sea/20101118105032" class="hatena-fotolife" target="_blank"><img src="http://cdn-ak.f.st-hatena.com/images/fotolife/d/d_sea/20101118/20101118105032.png?w=830" alt="f:id:d_sea:20101118105032p:image:left" title="f:id:d_sea:20101118105032p:image:left" class="hatena-fotolife hatena-image-left" data-recalc-dims="1" /></a>
  </p>
  
  <p>
    CLOUD EXPO はクラウド上で展開しているサービスの見本市というよりかは、完全にエンタープライズを意識したITベンダーの宣伝の場になっていた。各セッションのスピーカーはスポンサー企業の担当者で、展示物もハードウエアもあり、自社のソフトウエアもあり、「クラウドにかこつけた」キーワードで今まで通りの自社製品のアピールをしている。
  </p>
  
  <p>
    スポンサー企業のたくさんお金を出している上位を見ると大手ITベンダーであり、クライド？と思ってしまうようなメンツだ。
  </p>
  
  <p>
    アメリカらしいちょっと変わったセッションといえば、スタートアップがクライドを活用するための指南、SaaSスタートアップを束ねるようなコミュニティの紹介というところだろうか。ただ、これらのセッションもスタートアップを顧客と狙うコンサルタントが話していて、スタートアップが自ら語る生きた情報ではない。
  </p>
  
  <p>
    基本的には日本のよくあるクラウドセミナーでITベンダーがアピールする傾向とまったく同じなことが分かった。その次の週に日本で行われたCLOUD EXPOのほうが、さくらの新サービス発表とかニフティクラウドにAWSの方が飛び入り参加したりして、実際見ていないがこっちのほうが面白そうに思えた。展示に関しても、USは広めの会議室1つだったので幕張の展示面積のほうがずっと広いしたくさん展示されている。
  </p>
  
  <p>
    気になるAmazonは一番下のランクのスポンサーで、配布資料もWebに乗せているセキュリティホワイトペーパーを印刷した物、という気合のなさがまる分かりという感じだった。
  </p>
  
  <p>
    CLOUD EXPOと言いつつ、エンタープライズに完全に絞ったイベントだということが分かった。次回は行くことはないだろう。
  </p>
  
  <p>
    一方、RightScale User Conferenceはユーザ向けに自社サービスの活用法などを実践的に紹介していた。
  </p>
  
  <p>
    しかし、DBのスケーリングの良い答えはRightScaleでも持っていないのだなぁ。悩ましい。。。
  </p>
  
  <p>
    こちらの方のメモを以下に記しておく。
  </p>
  
  <p>
    <a name="seeall"></a>
  </p>
  
  <h5>
    RightScale Case Studies
  </h5>
  
  <p>
    RightScale を使っている4社が自ら説明
  </p>
  
  <ul>
    <li>
      American Gril</p> <ul>
        <li>
          子供向け人形、着替えなどの販売
        </li>
        <li>
          オンラインもやっている Jun 2010 スタート アバター使う
        </li>
        <li>
          Cloudへ移行した パフォーマンスを測って選定したよ
        </li>
        <li>
          1日で移行した implemented
        </li>
        <li>
          2500%キャパシティ increased
        </li>
        <li>
          すでに登録済みのユーザアプリケーションは1ヶ月後に移行した <ul>
            <li>
              1000%キャパシティ increased
            </li>
          </ul>
        </li>
        
        <li>
          構成&#160;: php CDN はAkamai <ul>
            <li>
              フロントは image, web array , re gstry jetty arrayなど
            </li>
          </ul>
        </li>
        
        <li>
          今後 much more work ahead <ul>
            <li>
              RightScaleでの教育を受けたい DEV, TEST, LOAD
            </li>
            <li>
              implement ability to auto-scale # 現状オートスケールは使ってないみたい
            </li>
          </ul>
        </li>
      </ul>
    </li>
    
    <li>
      WATCHMEN (AltEgo) <ul>
        <li>
          MMO iPhone Appらしい
        </li>
        <li>
          3DバーチャルなMMOぽい
        </li>
        <li>
          25日で 沢山のリクエストが来たよ、登録数ではないけど
        </li>
      </ul>
    </li>
    
    <li>
      なぜRightScale? <ul>
        <li>
          Fail Forward</p> <ul>
            <li>
              Nominalizes downtime, Unix Script
            </li>
            <li>
              RIghtScale Server Templatesすごくよいよ <ul>
                <li>
                  テストのためのたくさんのサーバの開発環境を用意
                </li>
                <li>
                  地域に関係なく利用出来る
                </li>
                <li>
                  RightScaleは早いレスポンスがよい
                </li>
              </ul>
            </li>
          </ul>
        </li>
      </ul>
    </li>
    
    <li>
      Associated Press <ul>
        <li>
          世界中にニュース配信する
        </li>
        <li>
          3700従業員、300ヶ所のロケーション
        </li>
        <li>
          ニュースにとってのチャレンジは第2のマーケットつくる
        </li>
        <li>
          色々なpublisherにライセンスニュースを送ること
        </li>
        <li>
          コンテンツのトラッキング、ニュースコンテンツのマネージメントとモニタリング
        </li>
        <li>
          今後 コンテンツライセンスの実行、Audience and engagement services
        </li>
        <li>
          Associated Pressから hNews を配信してトラッキングして、Real time rollup,Mapに描いて、Data Warehouse に食わせて News Registry として保存する <ul>
            <li>
              AWSはEC2, S3, CloudFront, CloudWatch
            </li>
            <li>
              AzureはTables, Queues, SQL Azure
            </li>
            <li>
              data ware house は自社、private cloud で開発、QA、Testした
            </li>
            <li>
              RightScaleのManagemnt UI使った。depoy時間が少ない、深い部分のマネージが出来る、コスト削減になった
            </li>
            <li>
              アクセス解析、アメリカ東側中心にアクセスあり
            </li>
          </ul>
        </li>
      </ul>
    </li>
    
    <li>
      Pogo.com <ul>
        <li>
          Socal Gameづくり、Facebook, iPhone, Yahoo!Gamesなど
        </li>
        <li>
          Free 100以上作っている、$5-30/m くらいのゲーム40種
        </li>
        <li>
          構成&#160;: AWS base でREST/jSON.HTTPSのフロントとSimpleDB, EBS, RDB, SQS, S3の連携 ＋ EA Hosted Oracle RAC
        </li>
        <li>
          テストが終わってからのデプロイの自動化が問題だった <ul>
            <li>
              installerを書いた
            </li>
            <li>
              Nexus から Automated Buikds(Hudson)つかって、Debian Repository(Jetty, JDK含む)でパッケージ化して、Ubuntu EC2にデプロイする流れ
            </li>
            <li>
              RightScaleのモニタリング使えるよ、いろいろな言語のスクリプトが使える
            </li>
            <li>
              Webnorogukara Syslogで集めて SDBにHadoopかましたろを置く
            </li>
            <li>
              S3におく そこから HIVEと連携する
            </li>
            <li>
              デプロイがダウンタイムなく早くなった、ロールバックが早くなった
            </li>
          </ul>
        </li>
      </ul>
    </li>
    
    <li>
      Web Filings <ul>
        <li>
          SECレポートのためのソリューションを提供
        </li>
        <li>
          GAEの環境ではいくつかのユーザのアプリが動かない
        </li>
        <li>
          RightScaleのpre-built script パッケージからはじめられる
        </li>
        <li>
          スケーリングのパラメータも設定できる
        </li>
        <li>
          ちょっと問題あったけど今はRightScaleのUbuntu imageでcheck outしている <ul>
            <li>
              tweak scriptを使っている
            </li>
          </ul>
        </li>
        
        <li>
          rebuild と redeploy が12時間以内で実現できるようになった
        </li>
        <li>
          In-HouseするよりもCloud使って色々できるようになった
        </li>
      </ul>
    </li>
  </ul>
  
  <h5>
    RightScale Non-MySQL
  </h5>
  
  <p>
    <a href="http://www.codefutures.com/" target="_blank">codeFutures</a>というDBのShardingの製品を持つ会社の説明
  </p>
  
  <ul>
    <li>
      shard の説明より、Each partition forms part of a shard
    </li>
    <li>
      The key to Database Sharding&#8230; Share Nothing ができる
    </li>
    <li>
      C0001のCはCustomerなのか？
    </li>
    <li>
      ASとDBの間にproxy をはさむ
    </li>
    <li>
      EC2上でスケーラブルに増やしたらリニアに書き込みパフォーマンスがあがった
    </li>
    <li>
      Sharding work: No contention between servers
    </li>
    <li>
      Sharding を利用した場合、CPUの利用率が上がって、wait が減ったよ
    </li>
    <li>
      Database Typeの比較 MySQL, Postgess, Oracle vs NoSQL(in memory0
    </li>
    <li>
      NoSQLでShardingを動かすためには S2にkey,objectをputして、S3からgetする <ul>
        <li>
          # ってことはS2,S3で同じデータを持てているということ？
        </li>
        <li>
          MySQLでも同じ構成
        </li>
      </ul>
    </li>
    
    <li>
      Cross-Shardの構成 <ul>
        <li>
          Client側でAgregateしている、Go Fishの命令を出す</p> <ul>
            <li>
              各サーバにブロードキャストする
            </li>
          </ul>
        </li>
      </ul>
    </li>
    
    <li>
      Sharidingは2台あればfailoverに対応できる、メモリは3つのDBががあること
    </li>
    <li>
      S1とS2を1つにすることもできる Elastic shards
    </li>
    <li>
      現在はMySQLのみでできる、将来的にはPostgreSQL, NoSQL, Caching対応
    </li>
  </ul>
  
  <h5>
    RightScale Web Scaling
  </h5>
  
  <p>
    Webのサーバ構成でのProxy, App, DBそれぞれのスケーリング方法紹介
  </p>
  
  <ul>
    <li>
      バランシングにてELBはTCPレベルのみ(古い情報であることを断っている)
    </li>
    <li>
      HAProxy + Apache もある
    </li>
    <li>
      Recommendationは Minimum of two load balancers <ul>
        <li>
          異なる Availability zone に置くこと
        </li>
        <li>
          m1.largeを使うと良いパフォーマンスをした
        </li>
      </ul>
    </li>
    
    <li>
      Application Server Tier <ul>
        <li>
          (RightScaleの)Autoscaling使うことを想定
        </li>
        <li>
          自動化してcodeを持ってくる仕組みが前提
        </li>
        <li>
          Triggers で Custom で connections と Application specific metricsがある <ul>
            <li>
              instance size m1.largeが良い選択肢
            </li>
            <li>
              m1.smallはtestにいいよ
            </li>
          </ul>
        </li>
      </ul>
    </li>
    
    <li>
      Caching Tier <ul>
        <li>
          databaseへのアクセスを減らす
        </li>
        <li>
          PHP + memcached の組みわせはいいよ
        </li>
        <li>
          メモリサイズによってインスタンスサイズが決まる
        </li>
        <li>
          手動でのスケーリングになる <ul>
            <li>
              TTLを気にする
            </li>
            <li>
              memcached
            </li>
          </ul>
        </li>
      </ul>
    </li>
    
    <li>
      DB Tire <ul>
        <li>
          slaveのスケールの時にMySQL Proxyを使う構成を紹介
        </li>
        <li>
          マルチマスターはライトスケールはお勧めしない
        </li>
        <li>
          NoSQLは一部のユーザで使っている <ul>
            <li>
              SDBとか
            </li>
          </ul>
        </li>
      </ul>
    </li>
    
    <li>
      DBのスケーリングのおすすめはない # あらあら。。。
    </li>
    <li>
      It depends!だそうな
    </li>
  </ul>
</div>