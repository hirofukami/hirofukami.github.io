---
title: UNIX/Linux bonding setting
date: Tue, 18 Sep 2007 15:00:00 +0000
author: Hiro Fukami
layout: post
permalink: /2007/09/18/unix-linux-bonding-setting/
tumblr_hirofukami_permalink:
  - http://hirofukami.tumblr.com/post/29609159343/unix-linux-bonding-setting
  - http://hirofukami.tumblr.com/post/29609159343/unix-linux-bonding-setting
  - http://hirofukami.tumblr.com/post/29609159343/unix-linux-bonding-setting
  - http://hirofukami.tumblr.com/post/29609159343/unix-linux-bonding-setting
tumblr_hirofukami_id:
  - 29609159343
  - 29609159343
  - 29609159343
  - 29609159343
original_post_id:
  - 419
  - 419
  - 419
  - 419
categories:
  - Tech
tags:
  - Network
  - Server
---
<div class="section">
  <p>
    検索しても意外とちゃんと情報がまとまっていなかったので、自分で整理メモ。
  </p>
  
  <h5>
    設定内容
  </h5>
  
  <ul>
    <li>
      mode はアクティブスタンバイ</p> <ul>
        <li>
          運用上の NIC の冗長化を狙っているので、定常時にトラフィックが流れる向きは固定したい
        </li>
      </ul>
    </li>
    
    <li>
      最短で切り替わるようにしたい miimon は最小 100msec
    </li>
  </ul>
  
  <h5>
    1) /etc/modprobe.conf
  </h5>
  
  <p>
    以下のように記述
  </p>
  
  <pre>

# vi /etc/modprobe.conf



alias bond0 bonding

options bond0 mode=1 primary=eth0

options bond0 miimon=100

</pre>
  
  <h5>
    2) ifcfg-bond0 を作成
  </h5>
  
  <p>
    記述形式は ifcfg-eth0 などと同様なので、コピってきたほうが早い。
  </p>
  
  <pre>

# cd /etc/sysconfig/network-scripts

# cp ifcfg-eth0 ifcfg-bond0

# vi ifcfg-bond0

</pre>
  
  <p>
    記述内容は
  </p>
  
  <pre>

DEVICE=bond0

IPADDR=192.168.1.100

NETMASK=255.255.255.0

NETWORK=192.168.1.0

BROADCAST=192.168.1.255

ONBOOT=yes

BOOTPROTO=none

</pre>
  
  <p>
    として、保存。
  </p>
  
  <h5>
    3) 対象となる物理インターフェイスへの設定 ifcfg-eth0, ifcfg-eth1 設定変更
  </h5>
  
  <pre>

# cd /etc/sysconfig/network-scripts

# vi ifcfg-eth0



DEVICE=eth0

ONBOOT=yes

MASTER=bond0

SLAVE=yes

BOOTPROTO=none

</pre>
  
  <pre>

# vi ifcfg-eth1



DEVICE=eth1

ONBOOT=yes

MASTER=bond0

SLAVE=yes

BOOTPROTO=none

</pre>
  
  <h5>
    4) network の再起動
  </h5>
  
  <pre>

# /etc/rc.d/init.d/network restart

</pre>
  
  <h5>
    5) 確認
  </h5>
  
  <ul>
    <li>
      ifconfig でみる
    </li>
  </ul>
  
  <p>
    bond0, eth0, eth1 ともに MAC アドレスが同じになっていて、bond0 &#8220;MASTER&#8221;, eth0,1 &#8220;SLAVE&#8221; となっていればOK
  </p>
  
  <p>
    この時、IPアドレスは bond0 にしかついていないことも確認
  </p>
  
  <pre>

bond0  	Link encap:Ethernet HWaddr 00:90:99:32:4F:DC

inet addr:172.16.100.31 Bcast:172.16.255.255 Mask:255.255.0.0

UP BROADCAST RUNNING MASTER MULTICAST MTU:1500 Metric:1

RX packets:210 errors:0 dropped:0 overruns:0 frame:0

TX packets:42 errors:0 dropped:0 overruns:0 carrier:0

collisions:0 txqueuelen:0

RX bytes:19458 (19.0 Kb) TX bytes:3162 (3.0 Kb)



eth0 	Link encap:Ethernet HWaddr 00:90:99:32:4F:DC

UP BROADCAST RUNNING SLAVE MULTICAST MTU:1500 Metric:1

RX packets:104 errors:0 dropped:0 overruns:0 frame:0

TX packets:21 errors:0 dropped:0 overruns:0 carrier:0

collisions:0 txqueuelen:100

RX bytes:9669 (9.4 Kb) TX bytes:1698 (1.6 Kb)

Interrupt:15 Base address:0xdf80



eth1 	Link encap:Ethernet HWaddr 00:90:99:32:4F:DC

UP BROADCAST RUNNING SLAVE MULTICAST MTU:1500 Metric:1

RX packets:106 errors:0 dropped:0 overruns:0 frame:0

TX packets:21 errors:0 dropped:0 overruns:0 carrier:0

collisions:0 txqueuelen:100

RX bytes:9789 (9.5 Kb) TX bytes:1464 (1.4 Kb)

Interrupt:10 Base address:0x1c00

</pre>
  
  <ul>
    <li>
      dmesg を実行
    </li>
  </ul>
  
  <p>
    タイムスタンプはないが bond0 で eth0, eth1 の active 切り替わりが確認できる
  </p>
  
  <ul>
    <li>
      tail /var/log/message を実行
    </li>
  </ul>
  
  <p>
    タイムスタンプありで bond0 の切り替わりがログに出力される
  </p>
  
  <p>
    <a href="http://www.stackasterisk.jp/tech/systemConstruction/teaming01_01.jsp" target="_blank">参考サイト</a>
  </p>
  
  <p>
    <a href="http://www.linux.or.jp/JF/JFdocs/kernel-docs-2.4/networking/bonding.txt.html" target="_blank">bonding記述の和訳ドキュメント</a>
  </p>
</div>