---
title: Amazon EC2 上で BGP peer を張ってみる
date: Mon, 14 Sep 2009 15:00:00 +0000
author: Hiro Fukami
layout: post
permalink: /2009/09/14/amazon-ec2-bgp-peer/
tumblr_hirofukami_permalink:
  - http://hirofukami.tumblr.com/post/29678344427/amazon-ec2-bgp-peer
  - http://hirofukami.tumblr.com/post/29678344427/amazon-ec2-bgp-peer
  - http://hirofukami.tumblr.com/post/29678344427/amazon-ec2-bgp-peer
tumblr_hirofukami_id:
  - 29678344427
  - 29678344427
  - 29678344427
original_post_id:
  - 215
  - 215
  - 215
categories:
  - Tech
tags:
  - AWS
  - BGP
  - ec2
  - Network
  - Server
  - tech
---
<div class="section">
  <p>
    <a href="http://d.hatena.ne.jp/d_sea/20090912/p1" target="_blank">hbstudy#3で話した</a>時にデモした、Amazon EC2 上で BGP peer を張る環境の作り方をメモしておく。
  </p>
  
  <h4>
    構成情報
  </h4>
  
  <p>
    環境は以下、
  </p>
  
  <ul>
    <li>
      EC2 の instance は CentOS5.0(Final)
    </li>
    <li>
      Quagga(旧Zebra)を使う
    </li>
  </ul>
  
  <p>
    構成は以下、
  </p>
  
  <p>
    <a href="http://f.hatena.ne.jp/d_sea/20090917093725" class="hatena-fotolife" target="_blank"><img src="http://cdn-ak.f.st-hatena.com/images/fotolife/d/d_sea/20090917/20090917093725.png?w=500" alt="f:id:d_sea:20090917093725p:image:w500" title="f:id:d_sea:20090917093725p:image:w500" class="hatena-fotolife" data-recalc-dims="1" /></a>
  </p>
  
  <ul>
    <li>
      instance 2つを立ち上げる
    </li>
    <li>
      Quagga 上で zebra, bgpd を起動
    </li>
    <li>
      AWS 内の private network 内部で BGP peering
    </li>
  </ul>
  
  <dl>
    <dt>
      instance 1 側の設定内容
    </dt>
    
    <dd>
      interface ip address&#160;: 10.254.202.228<br />AS65001<br />広報するprefix: 10.1.0.0/16、10.11.0.0./16、10.111.0.0/16
    </dd>
    
    <dt>
      instance 2 側の設定内容
    </dt>
    
    <dd>
      interface ip address&#160;: 10.209.162.213<br />AS65002<br />広報するprefix: 10.2.0.0/16
    </dd>
  </dl>
  
  <dl>
    <dt>
      BGP的な設定内容
    </dt>
    
    <dd>
      2つのinstance の interface ip address が異なるネットワークになるので、EBGP multihop を設定<br />route-map を使って Local Preference、MED、Community を付けてみる
    </dd>
  </dl>
  
  <dl>
    <dt>
      確認内容
    </dt>
    
    <dd>
      peerが張れているか<br />広報されている経路情報が route-map で指定しているものかどうか
    </dd>
  </dl>
  
  <h4>
    構築
  </h4>
  
  <p>
    CentOS だと yum で quagga がインストールできるので、簡単にすませる。
  </p>
  
  <pre>

[root@domU-12-31-39-07-A1-27:~] yum -y install quagga

</pre>
  
  <p>
    /etc/quagga 配下にファイルが作られる
  </p>
  
  <pre>

[root@domU-12-31-39-07-A1-27:~] cd /etc/quagga/

[root@domU-12-31-39-07-A1-27:/etc/quagga]ll

total 48

-rw-r--r-- 1 root   root      570 May 30  2007 bgpd.conf.sample

-rw-r--r-- 1 root   root     2801 May 30  2007 bgpd.conf.sample2

-rw-r--r-- 1 root   root     1110 May 30  2007 ospf6d.conf.sample

-rw-r--r-- 1 root   root      182 May 30  2007 ospfd.conf.sample

-rw-r--r-- 1 root   root      410 May 30  2007 ripd.conf.sample

-rw-r--r-- 1 root   root      394 May 30  2007 ripngd.conf.sample

-rwxr-x--- 1 quagga quaggavt  128 May 30  2007 vtysh.conf.sample

-rw-r--r-- 1 root   root      373 May 30  2007 zebra.conf.sample

-rw-r----- 1 quagga quagga     32 Sep 12 03:32 zebra.conf.sample02

</pre>
  
  <p>
    zebra.conf と bgpd.conf を以下のように書いた。bgpd.confのほうで空白行は必ず &#8220;!&#8221; を記述しないと動作してくれなかったので注意。
  </p>
  
  <p>
    instance1の場合
  </p>
  
  <p>
    zebra.conf
  </p>
  
  <pre>

[root@domU-12-31-39-07-A1-27:/etc/quagga] more zebra.conf

! -*- zebra -*-

!

! zebra sample configuration file

!

! $Id: zebra.conf.sample,v 1.1.1.1 2002/12/13 20:15:30 paul Exp $

!

hostname Router

password zebra # ここは変える

enable password zebra # ここは変える

!

! Interface's description.

!

!interface lo

! description test of desc.

!

!interface sit0

! multicast



!

! Static default route sample.

!

!ip route 0.0.0.0/0 203.181.89.241

!



!log file zebra.log

</pre>
  
  <p>
    bgpd.conf
  </p>
  
  <pre>

[root@domU-12-31-39-00-C5-16:~] more /usr/local/etc/bgpd.conf

! -*- bgp -*-

!

! BGPd sample configuratin file

!

! $Id: bgpd.conf.sample,v 1.1.1.1 2002/12/13 20:15:29 paul Exp $

!

hostname bgpd

password zebra

!enable password please-set-at-here

!

!bgp mulitple-instance

!

router bgp 65001

! bgp router-id 10.254.202.228

network 10.1.0.0/16

network 10.11.0.0/16

network 10.111.0.0/16

neighbor 10.209.162.213 remote-as 65002

neighbor 10.209.162.213 route-map TEST-ROUTEMAP-IN in

neighbor 10.209.162.213 route-map TEST-ROUTEMAP-OUT out

neighbor 10.209.162.213 ebgp-multihop

! neighbor 10.0.0.2 next-hop-self

!

! access-list all permit any

!

route-map TEST-ROUTEMAP-IN permit 10

set local-preference 200

set community 65001:65002

!

route-map TEST-ROUTEMAP-OUT permit 10

set metric 111

!

!

!route-map set-nexthop permit 10

! match ip address all

! set ip next-hop 10.0.0.1

!

!log file bgpd.log

!

log stdout

</pre>
  
  <p>
    instance2の場合
  </p>
  
  <p>
    zebra.conf は内容が同じなので、省略。
  </p>
  
  <p>
    bgpd.conf
  </p>
  
  <pre>

[root@domU-12-31-39-07-A1-27:/etc/quagga] more bgpd.conf

! -*- bgp -*-

!

! BGPd sample configuratin file

!

! $Id: bgpd.conf.sample,v 1.1.1.1 2002/12/13 20:15:29 paul Exp $

!

hostname bgpd

password zebra

!enable password please-set-at-here

!

!bgp mulitple-instance

!

router bgp 65002

! bgp router-id 10.209.162.213

network 10.2.0.0/16

neighbor 10.254.202.228 remote-as 65001

! neighbor 10.0.0.2 route-map set-nexthop out

neighbor 10.254.202.228 ebgp-multihop

! neighbor 10.0.0.2 next-hop-self

!

! access-list all permit any

!

!route-map set-nexthop permit 10

! match ip address all

! set ip next-hop 10.0.0.1

!

!log file bgpd.log

!

log stdout

</pre>
  
  <h4>
    起動
  </h4>
  
  <p>
    起動する。bgpを使うので、zebraとbgpd。
  </p>
  
  <pre>

[root@domU-12-31-39-07-A1-27:/etc/quagga] zebra -d

[root@domU-12-31-39-07-A1-27:/etc/quagga] bgpd -d

</pre>
  
  <p>
    zebra(2601)とbgpd(2605)のポートが空く
  </p>
  
  <pre>

root@domU-12-31-39-07-A1-27:/etc/quagga] netstat -an | grep 260*

tcp        0      0 0.0.0.0:2601                0.0.0.0:*                   LISTEN

tcp        0      0 0.0.0.0:2605                0.0.0.0:*                   LISTEN

</pre>
  
  <h4>
    確認
  </h4>
  
  <p>
    bgpd にログインして確認してみる。instance2 側にて。
  </p>
  
  <pre>

[root@domU-12-31-39-07-A1-27:/etc/quagga] telnet localhost bgpd

</pre>
  
  <p>
    まずは peer が張れているかどうか確認
  </p>
  
  <pre>

bgpd# show ip bgp su

BGP router identifier 10.209.162.213, local AS number 65002

2 BGP AS-PATH entries

0 BGP community entries



Neighbor        V    AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd

10.254.202.228  4 65001       7       8        0    0    0 00:04:34        3



Total number of neighbors 1

</pre>
  
  <p>
    張れている。続いて、経路情報を確認する。
  </p>
  
  <pre>

bgpd# show ip bgp

BGP table version is 0, local router ID is 10.209.162.213

Status codes: s suppressed, d damped, h history, * valid, &gt; best, i - internal

Origin codes: i - IGP, e - EGP, ? - incomplete



Network          Next Hop            Metric LocPrf Weight Path

*&gt; 10.1.0.0/16      10.254.202.228         111             0 65001 i

*&gt; 10.2.0.0/16      0.0.0.0                  0         32768 i

*&gt; 10.11.0.0/16     10.254.202.228         111             0 65001 i

*&gt; 10.111.0.0/16    10.254.202.228         111             0 65001 i



Total number of prefixes 4

</pre>
  
  <p>
    AS65001から3prefixがMED111で広報されていていることが分かる。
  </p>
  
  <p>
    instance1側で経路情報を見ると、
  </p>
  
  <pre>

bgpd# show ip bgp

BGP table version is 0, local router ID is 10.254.202.228

Status codes: s suppressed, d damped, h history, * valid, &gt; best, i - internal

Origin codes: i - IGP, e - EGP, ? - incomplete



Network          Next Hop            Metric LocPrf Weight Path

*&gt; 10.1.0.0/16      0.0.0.0                  0         32768 i

*&gt; 10.2.0.0/16      10.209.162.213           0    200      0 65002 i

*&gt; 10.11.0.0/16     0.0.0.0                  0         32768 i

*&gt; 10.111.0.0/16    0.0.0.0                  0         32768 i



Total number of prefixes 4

</pre>
  
  <p>
    AS65002からの経路情報に local preference 200 がついていることが分かる。
  </p>
  
  <p>
    communityを見ると、
  </p>
  
  <pre>

bgpd# show ip bgp community 65001:65002

BGP table version is 0, local router ID is 10.254.202.228

Status codes: s suppressed, d damped, h history, * valid, &gt; best, i - internal

Origin codes: i - IGP, e - EGP, ? - incomplete



Network          Next Hop            Metric LocPrf Weight Path

*&gt; 10.2.0.0/16      10.209.162.213           0    200      0 65002 i



Total number of prefixes 1

</pre>
  
  <p>
    AS65002からの経路情報に community 65001:65002 がついていることが分かる。
  </p>
  
  <p>
    ということで、めでたく BGP が張れて経路情報に属性を付けれましたと。
  </p>
</div>