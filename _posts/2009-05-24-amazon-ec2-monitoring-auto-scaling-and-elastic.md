---
title: Amazon EC2 新機能 Monitoring, Auto Scaling and Elastic Load Balancing を一通り触ってみた
date: Sun, 24 May 2009 15:00:00 +0000
author: Hiro Fukami
layout: post
permalink: /2009/05/24/amazon-ec2-monitoring-auto-scaling-and-elastic/
tumblr_hirofukami_permalink:
  - http://hirofukami.tumblr.com/post/29678335541/amazon-ec2-monitoring-auto-scaling-and-elastic
  - http://hirofukami.tumblr.com/post/29678335541/amazon-ec2-monitoring-auto-scaling-and-elastic
  - http://hirofukami.tumblr.com/post/29678335541/amazon-ec2-monitoring-auto-scaling-and-elastic
tumblr_hirofukami_id:
  - 29678335541
  - 29678335541
  - 29678335541
original_post_id:
  - 235
  - 235
  - 235
categories:
  - Tech
tags:
  - auto scaling
  - AWS
  - CloudWatch
  - ELB
  - report
  - Server
  - tech
---
<div class="section">
  <p>
    5/18に Amazon AWS から<a href="http://aws.typepad.com/aws/2009/05/new-aws-load-balancing-automatic-scaling-and-cloud-monitoring-services.html" target="_blank">発表になった</a> Amazon EC2 の新機能は Auto Scaling や Load Blanacing など、今までユーザ側でなんとかしていた or 提供事業者が飯の種にしていた部分に Amazon がカバーしてしまった。と言う点で結構注目が集まっていた。
  </p>
  
  <p>
    細かな点はさておき、ひとまず触ってみないことにはどこまでカバーされているかも分からないので触ってみた。以下、触ってみたときのメモ。
  </p>
  
  <h4>
    事前情報
  </h4>
  
  <p>
    設定の仕方やコマンドはそれぞれのページを参考にした。
  </p>
  
  <ul>
    <li>
      <a href="http://aws.amazon.com/cloudwatch/" target="_blank">Amazon CloudWatch</a>
    </li>
    <li>
      <a href="http://aws.amazon.com/autoscaling/" target="_blank">Auto Scaling</a>
    </li>
    <li>
      <a href="http://aws.amazon.com/elasticloadbalancing/" target="_blank">Elastic Load Balancing</a>
    </li>
  </ul>
  
  <h5>
    3機能概要
  </h5>
  
  <ul>
    <li>
      CloudWatch は instance と Elastic Load Balancer 両方のモニタリングが行える</p> <ul>
        <li>
          それぞれ取得できるパラメータは異なる
        </li>
      </ul>
    </li>
    
    <li>
      Auto Scaling が動作するためには、CloudWatch が instance 上で有効になっていることが条件
    </li>
    <li>
      Elastic Load Balancing は起動している instance を指定して分散対象に加えることができる
    </li>
    <li>
      beta版ということで、すべてコマンドラインでの操作方法しか提供されていない <ul>
        <li>
          Elasticfox 上だと今回の機能に対する情報は見えない
        </li>
      </ul>
    </li>
  </ul>
  
  <h4>
    動作環境
  </h4>
  
  <ul>
    <li>
      すでに EC2 コマンドが実施できる環境があることを前提
    </li>
    <li>
      mac 上でオペレーションしています
    </li>
  </ul>
  
  <h4>
    事前準備
  </h4>
  
  <ul>
    <li>
      Amazon AWS web へログイン
    </li>
    <li>
      Amazon EC2 API Tools をダウンロードして、ローカルにて解凍
    </li>
    <li>
      EC2 コマンドへの PATH が通っている既存ディレクトリと置き換える <ul>
        <li>
          今回の3機能追加にともなうもろもろのコマンドが増えている
        </li>
      </ul>
    </li>
  </ul>
  
  <h4>
    CloudWatch を触る
  </h4>
  
  <h5>
    事前準備
  </h5>
  
  <p>
    README.TXT をみつつ
  </p>
  
  <ul>
    <li>
      Amazon CloudWatch API Tools をダウンロードローカルに配置
    </li>
    <li>
      .bash_profile に PATH を追加
    </li>
    <li>
      credential-file-path.template をコピーして好きな名前に変える <ul>
        <li>
          今回は credential-file とした
        </li>
      </ul>
    </li>
    
    <li>
      このファイルに必要な情報を書く <ul>
        <li>
          key.id と secret key の情報は AWS web の my page よりコピペする
        </li>
      </ul>
    </li>
    
    <li>
      .bash_profile に追記した中身
    </li>
  </ul>
  
  <pre>

export AWS_CLOUDWATCH_HOME=~/Dropbox/AmazonAWS/CloudWatch

export PATH=$PATH:$AWS_CLOUDWATCH_HOME/bin

export AWS_CREDENTIAL_FILE=$AWS_CLOUDWATCH_HOME/credential-file

</pre>
  
  <ul>
    <li>
      以下の記述があらかじめ入っていること
    </li>
  </ul>
  
  <pre>

export EC2_PRIVATE_KEY=$EC2_HOME/pk-****.pem

export EC2_CERT=$EC2_HOME/cert-****.pem

</pre>
  
  <ul>
    <li>
      確認 mon-** コマンドが実行できるようになっていること
    </li>
  </ul>
  
  <h5>
    動かしてみる
  </h5>
  
  <ul>
    <li>
      EC2 の instance を起動する時に CloudWatch のモニターを有効にする</p> <ul>
        <li>
          コマンドラインで -m オプションを付けて起動
        </li>
        <li>
          もしくは instance 起動後に instance-ID を指定して、
        </li>
      </ul>
    </li>
  </ul>
  
  <pre>

$ ec2-monitor-instances i-***

</pre>
  
  <p>
    を実行する
  </p>
  
  <ul>
    <li>
      モニターできるパラメータ 09.05.24 現在
    </li>
  </ul>
  
  <dl>
    <dt>
      EC2 instance 用
    </dt>
    
    <dd>
      CPUUtilization<br />DiskReadBytes<br />DiskReadOps<br />DiskWriteBytes<br />DiskWriteOps<br />NetworkIn<br />NetworkOut
    </dd>
    
    <dt>
      Elastic Load Balancer 用
    </dt>
    
    <dd>
      HealthyHostCount<br />Latency<br />RequestCount<br />UnHealthyHostCount
    </dd>
  </dl>
  
  <ul>
    <li>
      stats を見るコマンド mon-get-stats
    </li>
  </ul>
  
  <p>
    コマンドのヘルプをみると、
  </p>
  
  <pre>

$ mon-get-stats -help



SYNOPSIS

mon-get-stats

MeasureName  --statistics  value[,value...] [--dimensions

"key1=value1,key2=value2..." ] [--end-time  value ] [--namespace  value

]

[--period  value ] [--start-time  value ] [--unit  value ]

[General Options]

</pre>
  
  <p>
    何が必須なのかが分かりづらいのと、key とか value に何を入れるかが説明不足。。。
  </p>
  
  <ul>
    <li>
      instance の CPU utilization をみるコマンド</p> <ul>
        <li>
          &#8212;start-time, &#8212;end-time, &#8212;namespace を設定しないとエラーが出ずに何も出力されなかった
        </li>
        <li>
          日本時間で見たいときは +09:00 を加えれば OK
        </li>
      </ul>
    </li>
  </ul>
  
  <pre>

$ mon-get-stats CPUUtilization --start-time 2009-05-22T15:00:00+09:00 --end-time 2009-05-22T15:30:00+09:00 --period 60 --statistics "Average,Minimum,Maximum" --namespace "AWS/EC2" --dimensions "InstanceId=i-1183c678"

2009-05-22 06:00:00  1.0  0     0.0   4.9E-324  Percent

2009-05-22 06:01:00  1.0  0.38  0.38  0.38      Percent

2009-05-22 06:02:00  1.0  0     0.0   4.9E-324  Percent

</pre>
  
  <ul>
    <li>
      Elastic Load Balancer の RequestCount をみるコマンド
    </li>
  </ul>
  
  <pre>

$ mon-get-stats RequestCount --namespace "AWS/ELB" --statistics "Average,Minimum,Maximum" --start-time 2009-05-25T12:00:00+09:00 --end-time 2009-05-25T13:30:00+09:00

2009-05-25 03:00:00  3.0  1  1.0  1.0  Count

2009-05-25 03:01:00  3.0  1  1.0  1.0  Count

2009-05-25 03:02:00  6.0  1  1.0  1.0  Count

2009-05-25 04:05:00  3.0  1  1.0  1.0  Count

</pre>
  
  <p>
    出力結果の数値はこの場合 Samples, Average, Minimum, Maximum のようだ
  </p>
  
  <h4>
    auto scaling を触る
  </h4>
  
  <h5>
    準備
  </h5>
  
  <ul>
    <li>
      CloudWatch の時と同じ
    </li>
    <li>
      auto scaling API tools をダウンロードして、.bash_profile で PATH 指定
    </li>
  </ul>
  
  <h5>
    動かしてみる
  </h5>
  
  <ul>
    <li>
      コマンドは as-** という形式
    </li>
  </ul>
  
  <ul>
    <li>
      1) as-create-launch-config&#160;: スケールアウトする時(増やす際)にどの AMI を増やして行くのかあらかじめ指定する
    </li>
    <li>
      2) as-create-auto-scaling-group&#160;: 先に設定した launch-config を group に設定する <ul>
        <li>
          &#8212;availablitity-zone と &#8212;min-size/&#8212;max-size は必須
        </li>
        <li>
          min-size は最小構成 instance 数、max-size は最大構成 instance 数
        </li>
        <li>
          1test という launch-config を使って、最小0/最大2 instance の test-group1 を作成する場合
        </li>
      </ul>
    </li>
  </ul>
  
  <pre>

$ as-create-auto-scaling-group test-group1 --launch-configuration 1test --availability-zones us-east-1a --min-size 0 --max-size 2

</pre>
  
  <ul>
    <li>
      3) as-create-or-update-trigger&#160;: auto-scaling-group に対する閾値設定</p> <ul>
        <li>
          test-group1 という auto-scaling group を使って、CPU利用率の平均で 20%以下 になった際にインスタンスを減らす、80% 以上になった時にインスタンスを増やす、120秒状態が継続した場合に発動する test-trigger2 という閾値を設定する場合
        </li>
      </ul>
    </li>
  </ul>
  
  <pre>

$ as-create-or-update-trigger test-trigger2 --auto-scaling-group test-group1 --namespace "AWS/EC2" --measure CPUUtilization --statistic Average --dimensions "AutoScalingGroupName=test-group01" --period 60 --lower-threshold 20 --lower-breach-increment=-1 --upper-breach-increment 1 --upper-threshold 80 --breach-duration 120

</pre>
  
  <ul>
    <li>
      応用編&#160;: auto-scaling group で min-size 1 にした場合</p> <ul>
        <li>
          group 内のすべての instance を terminated したら、自動的に 1 instance が起動する
        </li>
        <li>
          この時は launch-config で指定した AMI が起動する
        </li>
        <li>
          すべて落ちても自動的に立ち上げてくれるので、web のフロントや front proxy 的な動きをするものになどに使えそう
        </li>
        <li>
          手動ですべて落としたいときは
        </li>
      </ul>
    </li>
  </ul>
  
  <pre>

$ as-terminate-instance-in-auto-scaling-group i-**** --decrement-desired-capacity

</pre>
  
  <p>
    を実行すると指定したサイズに関係なく落ちてくれるので、自動起動はなくなる
  </p>
  
  <h4>
    load blancer を触る
  </h4>
  
  <h5>
    準備
  </h5>
  
  <ul>
    <li>
      CloudWatch の時と同じ
    </li>
    <li>
      Elastic Load Balancing API Tools をダウンロードして、.bash_profile で PATH 指定
    </li>
  </ul>
  
  <h5>
    動かしてみる
  </h5>
  
  <ul>
    <li>
      コマンドは elb-** という形式
    </li>
    <li>
      1) elb-create-lb&#160;: Load Balancer を作るコマンド <ul>
        <li>
          elb-create-lb -help をみる
        </li>
      </ul>
    </li>
  </ul>
  
  <pre>

SYNOPSIS

elb-create-lb

LoadBalancerName  --availability-zones  value[,value...]  --listener

"protocol=value, lb-port=value, instance-port=value" [ --listener

"protocol=value, lb-port=value, instance-port=value" ...]

[General Options]

</pre>
  
  <ul>
    <ul>
      <li>
        Load Balancer がサービスするポート番号と転送先ポート番号を設定する
      </li>
      <li>
        test-lb4 という名で、http 80 番でサービスして、http 80 番へ転送する Load Balancer を作成
      </li>
    </ul>
  </ul>
  
  <pre>

$ elb-create-lb test-lb4 --availability-zones us-east-1a --listener "lb-port=80,instance-port=80,protocol=http"

DNS-NAME  test-lb4-1200388198.us-east-1.elb.amazonaws.com

</pre>
  
  <ul>
    <ul>
      <li>
        出力結果に Load Balancer に割り当てる FQDN が発行される
      </li>
      <li>
        実際オリジナルドメインに結びつける時には、オリジナルドメインの DNS サーバ上で CNAME する必要があるだろう
      </li>
      <li>
        確かめてみると
      </li>
    </ul>
  </ul>
  
  <pre>

$ host test-lb4-1200388198.us-east-1.elb.amazonaws.com

test-lb4-1200388198.us-east-1.elb.amazonaws.com has address 174.129.197.180

</pre>
  
  <p>
    確かにグローバルアドレスが振られている
  </p>
  
  <ul>
    <li>
      2) elb-register-instances-with-lb&#160;: 作った ELB に instance を登録する</p> <ul>
        <li>
          すでにinstance name が分かっている(起動している)こと
        </li>
        <li>
          先ほど設定した test-lb4 という Load Balancer に3つの instance を登録する
        </li>
      </ul>
    </li>
  </ul>
  
  <pre>

$ elb-register-instances-with-lb test-lb4 --instances i-fb155392,i-ff155396,i-f315539a

INSTANCE-ID  i-fb155392

INSTANCE-ID  i-ff155396

INSTANCE-ID  i-f315539a

</pre>
  
  <ul>
    <li>
      3) Load Balancer は分散対象となる instance の health check を行っているので、確認</p> <ul>
        <li>
          Load Balancer 名 test-lb4 での health check 結果
        </li>
      </ul>
    </li>
  </ul>
  
  <pre>

$ elb-describe-instance-health test-lb4

INSTANCE-ID  i-fb155392  InService

INSTANCE-ID  i-ff155396  InService

INSTANCE-ID  i-f315539a  InService

</pre>
  
  <ul>
    <ul>
      <li>
        instance が生きていれば InService、死んでいれば OutService となる
      </li>
      <li>
        instance を登録した時点で、デフォルトで何がしかのチェックが行われている。設定内容は不明
      </li>
    </ul>
    
    <li>
      4) さらにオリジナルの health check設定を入れてみる <ul>
        <li>
          Load Balancer test-lb4 にて登録した instance に対して TCP 80 番ポートのチェック、interval 5sec(これが最小値)、timeout 3sec、死活の閾値 2回続いた時
        </li>
      </ul>
    </li>
  </ul>
  
  <pre>

$ elb-configure-healthcheck test-lb4 --target "TCP:80" --interval 5 --timeout 3 --unhealthy-threshold 2 --healthy-threshold 2

HEALTH-CHECK  TCP:80  5  3  2  2



$ elb-describe-instance-health test-lb4

INSTANCE-ID  i-fb155392  InService

INSTANCE-ID  i-ff155396  InService

INSTANCE-ID  i-f315539a  InService

</pre>
  
  <p>
    これにて確認終了。
  </p>
  
  <ul>
    <ul>
      <li>
        分散方式は不明 ソースIP縛りなのか、完全なラウンドロビンなのか
      </li>
      <li>
        設定内容的に L4 スイッチとしての振る舞いまで、L7レベルは設定できない
      </li>
    </ul>
  </ul>
  
  <h5>
    ちょっとはまったこと
  </h5>
  
  <ul>
    <li>
      Load Balancer から instance への health check する際には、instance に適用している sercurity group(firewall) にて source CIDR に <del datetime="2010-03-08T16:16:00+09:00">0.0.0.0/0(any)</del> ヘルスチェックしてくる source IP address が設定されていること
    </li>
    <li>
      health check packet がはじかれてしまい、OutService 扱いになってしまうため
    </li>
    <li>
      Load Balancer と instance に割り当てられていたグローバルアドレスより 174.129.197.0/24 を設定しても NG だったことから、異なる interface から通信していて、Amaozn EC2 内であってもすべて Security Group が適用されていると思われる
    </li>
    <li>
      => 任意のローカルアドレスからチェックされていることが分かった。tcpdump などしてアドレスを特定して security group に追加すれば広く設定する必要はない(<a href="http://d.hatena.ne.jp/d_sea/20100305/p1" target="_blank"> Amazon EC2 Elastic Load Balancing をもう少し詳しく見てみた &#8211; つれづれなる・・・</a><a href="http://b.hatena.ne.jp/entry/http://d.hatena.ne.jp/d_sea/20100305/p1" class="http-bookmark" target="_blank"><img src="http://b.hatena.ne.jp/entry/image/http://d.hatena.ne.jp/d_sea/20100305/p1" alt="" class="http-bookmark" /></a> より)
    </li>
  </ul>
  
  <h5>
    気になったこと
  </h5>
  
  <ul>
    <li>
      直接 instance に割り当てられた FQDN にブラウザからアクセスしてもみれる
    </li>
    <li>
      Load Balancer に割り当てられた FQDN にブラウザからアクセスしてもみれる
    </li>
    <li>
      instance のデフォルトゲートウエイが変わっていないことから、Load Balancer の動作は route 方式でなくて、おそらく NAT 方式なのだろう
    </li>
  </ul>
  
  <h4>
    全体通して分かったこと
  </h4>
  
  <p>
    (<a href="http://d.hatena.ne.jp/d_sea/20100306/p1" target="_blank"> Amazon EC2 Auto Scaling をもう少し詳しく見てみた &#8211; つれづれなる・・・</a><a href="http://b.hatena.ne.jp/entry/http://d.hatena.ne.jp/d_sea/20100306/p1" class="http-bookmark" target="_blank"><img src="http://b.hatena.ne.jp/entry/image/http://d.hatena.ne.jp/d_sea/20100306/p1" alt="" class="http-bookmark" /></a>より、正しくない部分を修正)
  </p>
  
  <h5>
    auto scaling と Load Balancer の連携は <del datetime="2010-03-08T22:22:38+09:00">今のところできてないようだ</del> 可能
  </h5>
  
  <ul>
    <li>
      自動的に auto scaling から Load Balancer へ instance-ID 情報を渡すコマンドなりは<del datetime="2010-03-08T22:22:38+09:00">見つけられなかった</del> &#8212;load-balancers オプションを用いる
    </li>
    <li>
      <del datetime="2010-03-08T22:22:38+09:00">Load Balancer の分散対象を設定するときはあらかじめ起動してある instance-ID を指定するので、auto scaling が立ち上げた instance-ID は手動で設定する必要がある</del>
    </li>
    <li>
      <del datetime="2010-03-08T22:22:38+09:00">Load Balancer 配下への追加は手動になるので、Load Balancer を使うケースは auto scale のメリットが発揮できない</del>
    </li>
    <li>
      <del datetime="2010-03-08T22:22:38+09:00">これができると web のフロントエンドの負荷分散を自動的にできて、auto scaling のメリットがでるが今回の機能追加ではカバーできていない</del>
    </li>
    <li>
      今回の機能追加 auto scaling は instance が落ちてしまった際、自動的に起動できる対障害性のアップにはなりそう
    </li>
  </ul>
  
  <h5>
    Amazon EC2 が制御できる範囲はあくまでネットワーク～電源 ON/OFF まで
  </h5>
  
  <ul>
    <li>
      Load Balancer は既存の L4 スイッチと変わらない基本動作をする
    </li>
    <li>
      ネットワーク機能的にはこれでもう整ったといっていいのでは
    </li>
    <li>
      AMI ベースで auto scaling するので、立ち上がった際の不整合は起きない環境で AMI 保存しておく必要がある
    </li>
    <li>
      auto scaling はあくまで instance の起動/終了になるので、OS 内でのオペレーションは関与できない
    </li>
    <li>
      バックエンドや DB のスケーリングをする際には OS 内のでのオペレーションが必要なので、ここは Amazon ではないサービス提供会社が行える部分と思われる
    </li>
  </ul>
</div>