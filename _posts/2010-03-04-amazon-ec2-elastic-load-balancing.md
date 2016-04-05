---
title: Amazon EC2 Elastic Load Balancing をもう少し詳しく見てみた
date: Thu, 04 Mar 2010 15:00:00 +0000
author: Hiro Fukami
layout: post
permalink: /2010/03/04/amazon-ec2-elastic-load-balancing/
tumblr_hirofukami_permalink:
  - http://hirofukami.tumblr.com/post/29678352015/amazon-ec2-elastic-load-balancing
  - http://hirofukami.tumblr.com/post/29678352015/amazon-ec2-elastic-load-balancing
  - http://hirofukami.tumblr.com/post/29678352015/amazon-ec2-elastic-load-balancing
tumblr_hirofukami_id:
  - 29678352015
  - 29678352015
  - 29678352015
original_post_id:
  - 199
  - 199
  - 199
categories:
  - Tech
tags:
  - AWS
  - ec2
  - ELB
  - Network
  - report
---
<div class="section">
  <p>
    Amazon EC2 の Elastic Load Balancing について、最初にまず触ってみたことは以下のエントリーに書いた。
  </p>
  
  <p>
    <a href="http://d.hatena.ne.jp/d_sea/20090525/p1" target="_blank"> Amazon EC2 新機能 Monitoring, Auto Scaling and Elastic Load Balancing を一通り触ってみた &#8211; つれづれなる・・・</a><a href="http://b.hatena.ne.jp/entry/http://d.hatena.ne.jp/d_sea/20090525/p1" class="http-bookmark" target="_blank"><img src="http://b.hatena.ne.jp/entry/image/http://d.hatena.ne.jp/d_sea/20090525/p1" alt="" class="http-bookmark" /></a>
  </p>
  
  <p>
    (このエントリーで分かったことを元に修正しています。)
  </p>
  
  <p>
    この時に疑問に思った点とか新しく気づいたことをのせます。
  </p>
  
  <h4>
    Load Balancer からのヘルスチェックがHTTP GETに対応していた
  </h4>
  
  <p>
    今までは単なるポートレベルでのチェックだったのが、html指定できるようになっている。
  </p>
  
  <p>
    Amazon Management Console から Load Balancer の設定ができるようになっているが、ヘルスチェックの画面でHTTP用にURLパスが設定できるようになっていた。
  </p>
  
  <p>
    「Ping Path」という部分が該当する。デフォルトは「index.html」になっている。
  </p>
  
  <p>
    <a href="http://f.hatena.ne.jp/d_sea/20100308123659" class="hatena-fotolife" target="_blank"><img src="http://cdn-ak.f.st-hatena.com/images/fotolife/d/d_sea/20100308/20100308123659.png?w=830" alt="f:id:d_sea:20100308123659p:image" title="f:id:d_sea:20100308123659p:image" class="hatena-fotolife" data-recalc-dims="1" /></a>
  </p>
  
  <p>
    ちなみに、プルダウンを「TCP」にすると、「Ping Path」が消える。HTTP以外で単純にポート番号に対してヘルスチェックを行う場合はこっちで設定する。
  </p>
  
  <p>
    <a href="http://f.hatena.ne.jp/d_sea/20100308123658" class="hatena-fotolife" target="_blank"><img src="http://cdn-ak.f.st-hatena.com/images/fotolife/d/d_sea/20100308/20100308123658.png?w=830" alt="f:id:d_sea:20100308123658p:image" title="f:id:d_sea:20100308123658p:image" class="hatena-fotolife" data-recalc-dims="1" /></a>
  </p>
  
  <h4>
    Load Balancer のヘルスチェックは AWS のローカルネットワーク内から任意のインスタンスで行われていた
  </h4>
  
  <p>
    通常、security group に最初に設定されている default には全てのポート番号に対して source IP address に default group が設定されている。これで自分が立ち上げたインスタンス間の通信は何でも通って、外部からのアクセスからは守られる状況が簡単に作れている。
  </p>
  
  <p>
    Amazon Management Console からは以下の画面で分かる。
  </p>
  
  <p>
    <a href="http://f.hatena.ne.jp/d_sea/20100308160819" class="hatena-fotolife" target="_blank"><img src="http://cdn-ak.f.st-hatena.com/images/fotolife/d/d_sea/20100308/20100308160819.png?w=830" alt="f:id:d_sea:20100308160819p:image" title="f:id:d_sea:20100308160819p:image" class="hatena-fotolife" data-recalc-dims="1" /></a>
  </p>
  
  <p>
    Elastic Load Balancing を利用するときに悩ましかったのが、ヘルスチェックパケットを送ってくる誰かは default group に該当しないらしく、Security Group でアドレスを追加して accept する必要があるけども、ヘルスチェックパケットを送ってくる source IP が画面上やコマンド上では分からないこと。
  </p>
  
  <p>
    Web用で利用するときは、0.0.0.0/0 で開けてしまうので問題ないのだけど、例えば MySQL のレプリケーションのスレーブ複数台を分散するために使いたい場合は any であけるのは、これはセキュリティ的によろしくない。
  </p>
  
  <p>
    実際、どの source IP から送ってくるのか tcpdump してみた。
  </p>
  
  <ul>
    <li>
      ヘルスチェックはHTTPのデフォルト値を採用
    </li>
    <li>
      起動したインスタンスで tcpdump を実施
    </li>
    <li>
      作成した Load Balancer は web-load-balancer-833540296.us-east-1.elb.amazonaws.com(IP address:184.73.238.21)
    </li>
  </ul>
  
  <pre>

[root@ip-10-242-119-83 ~]# tcpdump -n port 80



22:03:42.962921 IP 10.212.66.130.55786 &gt; 10.242.119.83.http: S 1970301109:1970301109(0) win 5840 &lt;mss 1460,sackOK,timestamp 1108158 0,nop,wscale 3&gt;

22:03:42.962970 IP 10.242.119.83.http &gt; 10.212.66.130.55786: S 2350734762:2350734762(0) ack 1970301110 win 5792 &lt;mss 1460,sackOK,timestamp 1345800047 1108158,nop,wscale 2&gt;

22:03:42.962981 IP 10.212.66.130.55776 &gt; 10.242.119.83.http: F 1:1(0) ack 1 win 730 &lt;nop,nop,timestamp 1108158 1345793614&gt;

22:03:42.963169 IP 10.242.119.83.http &gt; 10.212.66.130.55776: F 1:1(0) ack 2 win 1448 &lt;nop,nop,timestamp 1345800047 1108158&gt;

22:03:42.963410 IP 10.212.66.130.55786 &gt; 10.242.119.83.http: . ack 1 win 730 &lt;nop,nop,timestamp 1108158 1345800047&gt;

22:03:42.963454 IP 10.212.66.130.55776 &gt; 10.242.119.83.http: . ack 2 win 730 &lt;nop,nop,timestamp 1108158 1345800047&gt;

</pre>
  
  <p>
    10.212.66.130 がヘルスチェックパケットを送って来ていた。10.242.119.83 は自分のインスタンス。
  </p>
  
  <p>
    AWS 内で名前を引くと、
  </p>
  
  <pre>

[root@ip-10-242-119-83 ~]# host 10.242.119.83

83.119.242.10.in-addr.arpa domain name pointer ip-10-242-119-83.ec2.internal.

</pre>
  
  <p>
    EC2のインスタンスにつけるホスト名のような感じに見える。なので、ヘルスチェックするのは Load Balancer 用に立ち上げられているインスタンスが行っているようだ。
  </p>
  
  <p>
    ちなみに、Load Balancer のローカルアドレスでは？と思ってインスタンス上から Load Balancer の FQDN を引いてみると、
  </p>
  
  <pre>

[root@ip-10-242-119-83 ~]# host web-load-balancer-833540296.us-east-1.elb.amazonaws.com

web-load-balancer-833540296.us-east-1.elb.amazonaws.com has address 184.73.238.21

</pre>
  
  <p>
    グローバルアドレスが返ってきているので、ヘルスチェックしているのは Load Balancer <del datetime="2010-10-28T22:42:15+09:00">とは別物</del>のインターナル側のIPアドレスのようだ。(追記: ヘルスチェックしているのは別物ではなく同一インスタンスだった、という<a href="http://d.hatena.ne.jp/d_sea/comment?date=20100305#c112920840" target="_blank">コメントからの指摘</a>があったので修正)
  </p>
  
  <p>
    ヘルスチェックを通すために security group を広くあけるのが嫌だなぁと思っている方は、このホストだけを accept するように security group を設定すればいい。<del datetime="2010-10-28T22:42:15+09:00">このアドレスはおそらく動的には変わらないと思うので。条件は tcpdump のようなパケットキャプチャができることになるけど。</del>のだけど、ELBがEC2と同じようにアドレスが変わる可能性もありそうという話もあるので、10.0.0.0/255.0.0.0の広めにあけておくのが無難になりそう。(追記: こちらも<a href="http://d.hatena.ne.jp/d_sea/comment?date=20100305#c112920840" target="_blank">コメントからの指摘</a>があったので修正)
  </p>
  
  <p>
    Load Balancer はネットワークレベルでの分散を簡単に行えるので、使いようによっては結構便利。今回のこのティップスでひとまずanyで開けない程度の環境で利用できる。
  </p>
</div>