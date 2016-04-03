---
title: Amazon CloudFront スループットを測ってみた
date: Wed, 14 Oct 2009 15:00:00 +0000
author: Hiro Fukami
layout: post
permalink: /2009/10/14/amazon-cloudfront/
tumblr_hirofukami_permalink:
  - http://hirofukami.tumblr.com/post/29678345419/amazon-cloudfront
  - http://hirofukami.tumblr.com/post/29678345419/amazon-cloudfront
  - http://hirofukami.tumblr.com/post/29678345419/amazon-cloudfront
tumblr_hirofukami_id:
  - 29678345419
  - 29678345419
  - 29678345419
original_post_id:
  - 213
  - 213
  - 213
categories:
  - Tech
tags:
  - AWS
  - CloudFront
  - Network
  - report
  - Server
  - tech
---
<div class="section">
  <p>
    Amazon EC2系の記事を書く際にちゃんと動く内容を示さないといけないと思い、実際動作させながら確認していった。その中でも Amazon CloudFront のスループット測定はデータとしてちゃんと取れたので、導入効果が分かりやすかった。
  </p>
  
  <p>
    実際どれくらいパフォーマンスに差が出るのか、ファイルのダウンロードのスループット結果は以下。
  </p>
  
  <ul>
    <li>
      ダウンロードに使ったファイルは &#8220;Office2004-1154UpdateJA.dmg&#8221; で 9.8MB ほどのファイル
    </li>
    <li>
      Amazon S3 上の &#8220;test&#8221; bucket 上にファイルを保存
    </li>
    <li>
      &#8220;test&#8221; bucket を CloudFront に適用させる
    </li>
    <li>
      割り当てられたダウンロード可能なサーバのそれぞれの FQDN は以下 <ul>
        <li>
          オリジンサーバ&#160;: hironobu-bucket.s3.amazonaws.com</p> <ul>
            <li>
              Region: Us-East なので、日本からだと太平洋を渡って、アメリカをwestからeastまで横断する経路になるはず
            </li>
          </ul>
        </li>
        
        <li>
          キャッシュサーバ&#160;: dql6mokzq0d1p.cloudfront.net
        </li>
        <li>
          オリジンは ****.s3.amazonaws.com, キャッシュサーバは ****.cloudfront.net という名前になるのが特徴
        </li>
      </ul>
    </li>
    
    <li>
      Mac からそれぞれのサーバの URL に対して curl コマンドでダウンロードする
    </li>
    <li>
      3回ほど測定
    </li>
  </ul>
  
  <p>
    <span style="font-weight:bold;">オリジンサーバからのダウンロード</span>
  </p>
  
```bash
dsea-MacBook:~ hironobu$ curl -o /dev/null -w time_total http://hironobu-bucket.s3.amazonaws.com/test/Office2004-1154UpdateJA.dmg

% Total    % Received % Xferd  Average Speed   Time    Time     Time  Current Dload  Upload   Total   Spent    Left  Speed

100  9.8M  100  9.8M    0     0   355k      0  0:00:28  0:00:28 --:--:--  736k

time_totaldsea-MacBook:~ hironobu$ curl -o /dev/null -w time_total http://hironobu-bucket.s3.amazonawsOffice2004-1154UpdateJA.dmg

% Total    % Received % Xferd  Average Speed   Time    Time     Time  Current Dload  Upload   Total   Spent    Left  Speed

100  9.8M  100  9.8M    0     0   370k      0  0:00:27  0:00:27 --:--:--  401k

time_totaldsea-MacBook:~ hironobu$ curl -o /dev/null -w time_total http://hironobu-bucket.s3.amazonawsOffice2004-1154UpdateJA.dmg

% Total    % Received % Xferd  Average Speed   Time    Time     Time  Current Dload  Upload   Total   Spent    Left  Speed

100  9.8M  100  9.8M    0     0   404k      0  0:00:24  0:00:24 --:--:--  377k
  
  <p>
    ばらつきがあるが、400kB = 3.2Mbps くらい。太平洋とアメリカを横断するからこんなもんでしょう。
  </p>
  
  <p>
    <span style="font-weight:bold;">キャッシュサーバからのダウンロード</span>
  </p>
  
```bash
dsea-MacBook:~ hironobu$ curl -o /dev/null -w time_total http://dql6mokzq0d1p.cloudfront.net/test/Office2004-1154UpdateJA.dmg

% Total    % Received % Xferd  Average Speed   Time    Time     Time  Current Dload  Upload   Total   Spent    Left  Speed

100  9.8M  100  9.8M    0     0  2364k      0  0:00:04  0:00:04 --:--:-- 2374k

time_totaldsea-MacBook:~ hironobu$ curl -o /dev/null -w time_total http://dql6mokzq0d1p.cloudfront.netce2004-1154UpdateJA.dmg

% Total    % Received % Xferd  Average Speed   Time    Time     Time  Current Dload  Upload   Total   Spent    Left  Speed

100  9.8M  100  9.8M    0     0  2370k      0  0:00:04  0:00:04 --:--:-- 2380k

time_totaldsea-MacBook:~ hironobu$ curl -o /dev/null -w time_total http://dql6mokzq0d1p.cloudfront.netce2004-1154UpdateJA.dmg

% Total    % Received % Xferd  Average Speed   Time    Time     Time  Current Dload  Upload   Total   Spent    Left  Speed

100  9.8M  100  9.8M    0     0  2344k      0  0:00:04  0:00:04 --:--:-- 2355k
```

  
  <p>
    大体、2300kB/s = 18.4Mbps くらい
  </p>
  
  <p>
    国内ならばもうちょっとでても良いかもだがこんなもんか。オリジンと比べて約6倍速い。
  </p>
  
  <p>
    AWS を利用するにはインスタンスの物理位置は US or EU になってしまうので、どうしても日本からアクセスする際のネットワーク遅延の問題は付いて回る。これが日本においたキャッシュサーバを利用すれば解消できてしまうのは、あまり解決策として注目されていないかもしれない。
  </p>
  
  <p>
    ただ、静的ファイルだけがキャッシュに置けるので、動的なページ生成には適用できない。
  </p>
  
  <p>
    確か、学びing さんが一緒にやった Amazon EC2/S3 実践セミナーの時に動的に生成する部分の容量はすごく小さいから、重たい画像などをどんどん CloudFront においてパフォーマンスを上げるノウハウを紹介していたな。
  </p>
  
  <p>
    AWS は単に Amazon EC2/S3 だけを利用するだけではそのメリットはあまり生かせていなくて、その周辺のサービスも含めて利用することで日本からでも十分パフォーマンスの良いサイトが提供できると思っている。
  </p>
  
  <p>
    ただ、これを利用するには AWS に対する独自なノウハウが必要で、よく分からないながらも手探りで経験しながら体得していった貴重な価値になるだろう。やってみてしまえばそれほど難しい世界ではないことが分かる。
  </p>
  
  <p>
    幸い日本にはこれを活かしきれるノウハウを持った人たちがまだ少ないと思うので、価値を高めてくれていると感じる。
  </p>
</div>