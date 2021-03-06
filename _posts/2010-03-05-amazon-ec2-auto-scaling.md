---
title: Amazon EC2 Auto Scaling をもう少し詳しく見てみた
date: Fri, 05 Mar 2010 15:00:00 +0000
author: Hiro Fukami
layout: post
permalink: /2010/03/05/amazon-ec2-auto-scaling/
tumblr_hirofukami_permalink:
  - http://hirofukami.tumblr.com/post/29678353329/amazon-ec2-auto-scaling
  - http://hirofukami.tumblr.com/post/29678353329/amazon-ec2-auto-scaling
  - http://hirofukami.tumblr.com/post/29678353329/amazon-ec2-auto-scaling
tumblr_hirofukami_id:
  - 29678353329
  - 29678353329
  - 29678353329
original_post_id:
  - 198
  - 198
  - 198
categories:
  - Tech
tags:
  - auto scaling
  - AWS
  - ec2
  - Server
---
<div class="section">
  <p>
    以前、Auto Scaling について触ってみた内容は以下のエントリーで書いた。
  </p>
  
  <p>
    <a href="http://d.hatena.ne.jp/d_sea/20090525/p1" target="_blank"> Amazon EC2 新機能 Monitoring, Auto Scaling and Elastic Load Balancing を一通り触ってみた &#8211; つれづれなる・・・</a><a href="http://b.hatena.ne.jp/entry/http://d.hatena.ne.jp/d_sea/20090525/p1" class="http-bookmark" target="_blank"><img src="http://b.hatena.ne.jp/entry/image/http://d.hatena.ne.jp/d_sea/20090525/p1" alt="" class="http-bookmark" /></a>
  </p>
  
  <p>
    それ以降、実際に使ってみて、正しくない部分に気づいたので、前のエントリーの修正をしつつ追加で分かったことを含めて書いておく。
  </p>
  
  <h4>
    Auto Scaling の設定手順をまとめてみる
  </h4>
  
  <p>
    Auto Scaling の設定はいくつかの設定を依存させて動作させているので、その整理を行う。
  </p>
  
  <p>
    設定すべき内容は以下3つになる。
  </p>
  
  <ol>
    <li>
      launch-config
    </li>
    <li>
      auto-scaling-group
    </li>
    <li>
      trigger
    </li>
  </ol>
  
  <p>
    順番に設定する必要があり、依存関係は launch-config > auto-scaling-group > trigger となっている。
  </p>
  
  <p>
    それぞれについて解説する。
  </p>
  
  <h5>
    launch-config
  </h5>
  
  <p>
    自動的にインスタンスが起動する際に必要な項目を設定する
  </p>
  
  <ul>
    <li>
      設定する項目</p> <ul>
        <li>
          LaunchConfigurationName: 名前
        </li>
        <li>
          AMI: 立ち上げたいAMIを指定する
        </li>
        <li>
          key pair: 必須ではない設定項目だが、これを設定しないと key を使った ssh login ができないため、設定した方がいい
        </li>
        <li>
          security group: 起動したインスタンスに適用する security group
        </li>
        <li>
          instance type: 起動するインスタンスのタイプ smallとかlargeとか
        </li>
      </ul>
    </li>
    
    <li>
      作成するコマンドは、as-create-launch-config
    </li>
    <li>
      例:
    </li>
  </ul>
  
  <pre>

as-create-launch-config TEST-CONFIG --image-id ami-89e809e0 --key temp-key --group Web-group --instance-type m1.small

</pre>
  
  <ul>
    <ul>
      <li>
        LaunchConfigurationName: TEST-CONFIG
      </li>
      <li>
        AMI: ami-89e809e0
      </li>
      <li>
        key pair: temp-key
      </li>
      <li>
        security group: Web-group
      </li>
      <li>
        instance type: m1.small
      </li>
    </ul>
  </ul>
  
  <h5>
    auto-scaling-group
  </h5>
  
  <p>
    起動したインスタンスを動作させるための全般的な設定
  </p>
  
  <ul>
    <li>
      設定する項目</p> <ul>
        <li>
          AutoScalingGroupName: 名前
        </li>
        <li>
          availability-zones: インスタンスをどの availability-zones で起動させるか
        </li>
        <li>
          launch-configuration: 適用させる launch-configuration 名
        </li>
        <li>
          min-size: 起動させるインスタンスの最小数。0以上で max と同じもしくは小さい値にすること。
        </li>
        <li>
          max-size: 起動させるインスタンスの最大数。10000より小さくて min と同じもしくは大きい値にすること。
        </li>
        <li>
          load-balancers: 起動したインスタンスを load balancer 配下に追加する。このオプションを付けない場合は load balancer 配下に入らずに起動される
        </li>
      </ul>
    </li>
    
    <li>
      作成するコマンドは、as-create-auto-scaling-group
    </li>
    <li>
      例:
    </li>
  </ul>
  
  <pre>

as-create-auto-scaling-group test-group1 --launch-configuration 1test --availability-zones us-east-1a --min-size 0 --max-size 2 --load-balancers Web-LB

</pre>
  
  <ul>
    <ul>
      <li>
        AutoScalingGroupName: test-group1
      </li>
      <li>
        availability-zones: us-east-1a
      </li>
      <li>
        launch-configuration: 1test
      </li>
      <li>
        min-size: 0
      </li>
      <li>
        max-size: 2
      </li>
      <li>
        load-balancers: Web-LB
      </li>
    </ul>
  </ul>
  
  <h5>
    trigger
  </h5>
  
  <p>
    auto-scaling-groupを発動させるしきい値の設定。(ちょっと分かりづらい。いまいち把握しきれていない。)
  </p>
  
  <ul>
    <li>
      設定する項目</p> <ul>
        <li>
          TriggerName: 名前
        </li>
        <li>
          auto-scaling-group: 適用させる auto-scaling-group 名
        </li>
        <li>
          measure: しきい値として用いるモニタリング項目。CloudWatchで取得できる項目が該当する。
        </li>
        <li>
          statistic: しきい値として用いる数字の Average, Sum, Minimum, Maximum のうちどれかを選ぶ
        </li>
        <li>
          dimensions: モニタリングで利用する設定を示す
        </li>
        <li>
          period: モニタリングする頻度
        </li>
        <li>
          lower-threshold: この値を下回った際にしきい値を超えた判断する。しきい値の下限
        </li>
        <li>
          upper-threshold: この値を上回った際にしきい値を超えた判断する。しきい値の上限
        </li>
        <li>
          lower-breach-increment: しきい値の下限をどれくらい下回ったらスケールするかを設定する。-1 を設定したときは「lower-breach-increment=-1」と書く
        </li>
        <li>
          upper-breach-increment: しきい値の上限をどれくらい上回ったらスケールするかを設定する。
        </li>
        <li>
          breach-duration: しきい値を超えた際にどれくらいの時間が継続したらtriggerを発動するか設定する
        </li>
      </ul>
    </li>
    
    <li>
      作成するコマンドは、as-create-or-update-trigger
    </li>
    <li>
      例:
    </li>
  </ul>
  
  <pre>

as-create-or-update-trigger WebApp-trigger1 --auto-scaling-group WebApp-group1 --measure CPUUtilization --statistic Average --dimensions "AutoScalingGroupName=WebApp-group1" --period 60 --lower-threshold 20 --upper-threshold 95 --lower-breach-increment=-1 --upper-breach-increment 1 --breach-duration 180

</pre>
  
  <ul>
    <ul>
      <li>
        TriggerName: WebApp-trigger1
      </li>
      <li>
        auto-scaling-group: WebApp-group1
      </li>
      <li>
        measure: CPUUtilization
      </li>
      <li>
        statistic: Average
      </li>
      <li>
        dimensions: &#8220;AutoScalingGroupName=WebApp-group1&#8221;
      </li>
      <li>
        period: 60
      </li>
      <li>
        lower-threshold: 20
      </li>
      <li>
        upper-threshold: 95
      </li>
      <li>
        lower-breach-increment: -1
      </li>
      <li>
        upper-breach-increment: 1
      </li>
      <li>
        breach-duration: 180
      </li>
    </ul>
  </ul>
  
  <p>
    <h4>
      Auto Scaling で自動で増えたインスタンスを Elastic Load Balancing 配下に追加できた
    </h4>
    
    <p>
      オプションの見落としだったわけですが、以前は自動的に増えたインスタンスは Elastic Load Balancing 配下に自動的に追加されないと思っていたが、ちゃんと追加できるオプションがあった。Load Balancer 配下に自動的に追加されなければオートスケーリングの意味無いじゃんと思っていたけど、さすがアマゾン、ちゃんとサポートしていた。
    </p>
    
    <p>
      オプションは、as-create-auto-scaling-group コマンドにある。ヘルプを見てみると、
    </p>
    
    <pre>

SYNOPSIS

as-create-auto-scaling-group

AutoScalingGroupName  --availability-zones  value[,value...]

--launch-configuration  value  --max-size  value  --min-size  value

[--cooldown  value ] [--load-balancers  value[,value...] ]

[General Options]



(snip)

SPECIFIC OPTIONS

--cooldown VALUE

Time (in seconds) between a successful scaling activity and succeeding

scaling activity.



-l, --launch-configuration VALUE

Name of existing launch configuration to use to launch new instances.

Required.



--load-balancers VALUE1,VALUE2,VALUE3...

List of existing load balancers. Load balancers must exist in Elastic

Load Balancing service, within the scope of the caller's AWS account.



</pre>
    
    <p>
      「&#8212;load-balancers」というのがあり、Load Balancer 名を書くことでその配下に追加されることが可能になる。事前に Load Balancer は作成してあることが必要。
    </p>
    
    <p>
      <h4>
        決して落ちないインスタンスを作ることができる
      </h4>
      
      <p>
        min-size 1以上であれば動作させることが可能。
      </p>
      
      <p>
        例えば、min-size 1 にした場合、
      </p>
      
      <ul>
        <li>
          意味としては「必ず1台立ち上がっている=決して落ちない」状況を作ることができる
        </li>
        <li>
          Webサーバみたいな必ず立ち上がっていて欲しい時には &#8212;min-size 1 にしておくと良い <ul>
            <li>
              この時は &#8212;load-balancers オプションを忘れずに。事前にWeb用ロードバランサを作成しておくこと
            </li>
            <li>
              Web用ロードバランサの FQDN を DNS で CNAME して、www.example.com とかと紐付けて置くこと前提
            </li>
          </ul>
        </li>
        
        <li>
          この設定をした途端に自動的にインスタンスが立ち上がる。後述の trigger がなくても動作する
        </li>
        <li>
          手動で立ち上がっているインスタンスを terminate して、0台の状態にしても自動的に起動してくる
        </li>
      </ul>
      
      <h5>
        決して落ちない状況で起動インスタンス数を0にしたい場合
      </h5>
      
      <ul>
        <li>
          「as-terminate-instance-in-auto-scaling-group」コマンドで &#8212;decrement-desired-capacity オプションを付けて実行してから、インスタンスIDを指定して実行する。その後、そのインスタンスを terminate すると自動起動せずに落ちる。
        </li>
        <li>
          内容としてはオートスケーリングの設定範囲からこのインスタンスを外すという意味合い <ul>
            <li>
              例:
            </li>
          </ul>
        </li>
      </ul>
      
      <pre>

as-terminate-instance-in-auto-scaling-group i-f7e9859f --decrement-desired-capacity

</pre>
      
      <h4>
        実際 Auto Scaling がちゃんと動くかどうか試してみる
      </h4>
      
      <h5>
        設定する
      </h5>
      
      <ul>
        <li>
          webサーバのオートスケーリングを想定
        </li>
        <li>
          起動したインスタンスは web-front という Elastic Load Balancer 配下に登録する
        </li>
        <li>
          min-size 1 として最低でも1台のインスタンスが起動させること
        </li>
        <li>
          トリガーのしきい値は、CPU平均利用率が95%を超えた場合に増えて、20%を下回った場合に減らす
        </li>
        <li>
          上記の状態が180秒継続したらオートスケーリングが実施される
        </li>
      </ul>
      
      <p>
        以下の順にコマンドを実行
      </p>
      
      <pre>

$ as-create-launch-config TEST-CONFIG -i ami-654ea20c -t m1.small --group default,SSH,Web --key temp-key

</pre>
      
      <pre>

$ as-create-auto-scaling-group test-group1 --launch-configuration TEST-CONFIG -z us-east-1a,us-east-1b,us-east-1c,us-east-1d --min-size 1 --max-size 10 --load-balancers web-front

</pre>
      
      <p>
        このコマンドを入力した時点で、min-size 1 が有効になって1台インスタンスが起動した。
      </p>
      
      <pre>

$ as-create-or-update-trigger WebApp-trigger1 --auto-scaling-group test-group1 --measure CPUUtilization --statistic Average --namespace "AWS/EC2" --dimensions "AutoScalingGroupName=test-group1" --period 60 --lower-threshold 20 --upper-threshold 95 --lower-breach-increment=-1 --upper-breach-increment 1 --breach-duration 180

</pre>
      
      <p>
        設定完了。
      </p>
      
      <h5>
        実際CPUを回してオートスケーリングするかどうか確認してみる
      </h5>
      
      <p>
        1台起動したインスタンス内で stress コマンドを実施する。(事前にstressをインストールして利用可能な状況にしてある前提)
      </p>
      
      <p>
        実行コマンドは以下
      </p>
      
      <pre>

# stress -c 1 --timeout 360s

</pre>
      
      <ul>
        <ul>
          <li>
            起動しているのはスモールインスタンスでCPUは1つなので、Load Average 1 を狙えばCPU利用率は100%になるだろう
          </li>
          <li>
            timeout 360s にしているのは、180sec でオートスケーリングされるはずなので、起動時間も考慮して若干長めにとった
          </li>
        </ul>
      </ul>
      
      <p>
        AWS Management Console にて確認する。
      </p>
      
      <ul>
        <li>
          1台目のインスタンスにsshログイン
        </li>
        <li>
          stressコマンドを実行
        </li>
        <li>
          最初に起動しているインスタンスの CloudWatch でのモニタリングで、CPU利用率のグラフをみる。
        </li>
        <li>
          CPU利用率が100%近くになっていることを確認する
        </li>
        <li>
          180秒くらい経過したときに自動的に1台のインスタンスが起動した
        </li>
        <li>
          Load Balancers より、web-front 配下に登録されているインスタンスが2台になっていることを確認
        </li>
        <li>
          stress コマンド実行が終了
        </li>
        <li>
          しばらく経過して、後から起動した2台目のインスタンスが自動的に terminate した
        </li>
        <li>
          1台のインスタンスのみ起動した状態に戻る
        </li>
        <li>
          Load Balancers より、web-front 配下に登録されているインスタンスが1台になっていることを確認
        </li>
        <li>
          確認終了
        </li>
      </ul>
      
      <p>
        ちゃんと動いた。
      </p>
      
      <p>
        Auto Scaling を使いこなせれば、運用の手間を省くことが出来そうな設定内容になっている。ただし、あくまでサーバの起動・停止をオートスケーリングで行うだけなので、内部のアプリケーションの整合性はユーザ側で事前にとっておく必要がある。
      </p>
      
      <p>
        例えば、立ち上げるAMIは自動起動でプロセスが起動する状態で保存しておかなくてはいけないし、Load Balancer 配下に追加した場合、単純なラウンドロビンで分散するので、セッション維持など複数台構成をとっても問題が生じないようにしておく必要もある。
      </p>
      
      <p>
        そういう意味ではユーザ側で事前の準備や検証が必要で、その後の運用状況もチェックするようにしておかないと実サービス運用は難しいだろう。
      </p></div>