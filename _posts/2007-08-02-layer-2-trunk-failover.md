---
title: Layer 2 trunk failover
date: Thu, 02 Aug 2007 15:00:00 +0000
author: Hiro Fukami
layout: post
permalink: /2007/08/02/layer-2-trunk-failover/
tumblr_hirofukami_permalink:
  - http://hirofukami.tumblr.com/post/29609155770/layer-2-trunk-failover
  - http://hirofukami.tumblr.com/post/29609155770/layer-2-trunk-failover
  - http://hirofukami.tumblr.com/post/29609155770/layer-2-trunk-failover
  - http://hirofukami.tumblr.com/post/29609155770/layer-2-trunk-failover
tumblr_hirofukami_id:
  - 29609155770
  - 29609155770
  - 29609155770
  - 29609155770
original_post_id:
  - 429
  - 429
  - 429
  - 429
categories:
  - Tech
tags:
  - failover
  - Layer2
  - Network
---
<div class="section">
  <p>
    サーバ収容用の Layer2 switch を物理的冗長構成にしてポートをケチる時にスパニングツリーを使いがちになるわけだけど、心理的にスパニングツリーは怖くてなるべくなら使いたくないと思ったときに、ちょうどいい解決策があった。
  </p>
  
  <p>
    ただし、これは<del datetime="2008-12-17T11:32:15+09:00">ブレードサーバのエンクロージャに付く</del> Cisco Catalyst <del datetime="2008-12-17T11:32:15+09:00">Blade Switch 3020 という専用 module でのお話なので汎用性は低い</del>で動作する<span class="footnote"><a href="#f1" name="fn1" title="Catalyst2960でも出来たのでCatalystなら出来ると思われる" target="_blank">*1</a></span>。
  </p>
  
  <p>
    Layer 2 trunk failover の動作内容は、upstrem, downstream という2つの trunk group(もちろんシングルでも良い)を対応付けておいて、[upstream<del datetime="2008-12-17T11:32:15+09:00">|downstream]</del> down を検出したら、もう一方の <del datetime="2008-12-17T11:32:15+09:00">[upstream|</del>downstream] も down させるというもの<span class="footnote"><a href="#f2" name="fn2" title="2008/12/17修正:動作するのはupstreamのlinkが変化したときだけだった。downstreamがdownしてもupstreamはdonwしない" target="_blank">*2</a></span>。
  </p>
  
  <p>
    こうすることで、link status の同期を取って L2-SW の上下(実際には下位のサーバとはエンクロージャ内での接続なので、そのつなぎこみが断するのは考えにくいが、)のネットワーク機器の冗長機能を生かすことが可能。
  </p>
  
  <p>
    物理冗長しているL2-SW間の渡りのケーブルがいらなくなるので、ループにならないし、SPT を使わずにすむ。
  </p>
  
  <p>
    前提としてサーバは2NIC持っていて、それぞれ L2-SW につながっていて、bonding の設定がされていること。
  </p>
  
  <ul>
    <li>
      config (2008/12/17追記)
    </li>
  </ul>
  
  <p>
    こんな感じで設定する。link state track 番号は増やせるので、VLANごとに設定できる。この場合はGi0/1がダウンしたらGi0/9も連動してダウンする。Gi0/2がダウンしたらGi0/10も連動してダウンする。
  </p>
  
  <pre>

link state track 1

link state track 2

...

interface GigabitEthernet0/1

switchport access vlan 50

switchport mode access

speed 1000

link state group 1 upstream

!

interface GigabitEthernet0/2

switchport access vlan 60

switchport mode access

speed 1000

link state group 2 upstream

...

interface GigabitEthernet0/9

switchport access vlan 50

switchport mode access

speed 1000

link state group 1 downstream

!

interface GigabitEthernet0/10

switchport access vlan 60

switchport mode access

speed 1000

link state group 2 downstream

</pre>
  
  <p>
    <ul>
      <li>
        参考URL:
      </li>
    </ul>
    
    <dl>
      <dt>
        Cisco Catalyst Blade Switch 3020 for HP Getting Started Guide
      </dt>
      
      <dd>
        <a href="http://www.cisco.com/en/US/products/ps6748/products_getting_started_guide09186a00806c38a8.html" target="_blank"><a href="http://www.cisco.com/en/US/products/ps6748/products_getting_started_guide09186a00806c38a8.html" target="_blank">http://www.cisco.com/en/US/products/ps6748/products_getting_started_guide09186a00806c38a8.html</a></a>
      </dd>
      
      <dt>
        Configuring EtherChannels and Layer 2 Trunk Failover and Link-State Tracking
      </dt>
      
      <dd>
        <del datetime="2008-12-17T11:32:15+09:00"><a href="http://www.cisco.com/en/US/products/ps6748/products_configuration_guide_chapter09186a00806c3477.html" target="_blank"><a href="http://www.cisco.com/en/US/products/ps6748/products_configuration_guide_chapter09186a00806c3477.html" target="_blank">http://www.cisco.com/en/US/products/ps6748/products_configuration_guide_chapter09186a00806c3477.html</a></a></del>
      </dd>
    </dl>
    
    <p>
      リンク切れ
    </p>
    
    <dl>
      <dt>
        Cisco Systems, Inc. Cisco Catalyst Blade Switch 3020 for HP Hardware Installation Guide
      </dt>
      
      <dd>
        <a href="http://www.cisco.com/en/US/products/ps6748/products_installation_guide_chapter09186a008065b0b9.html" target="_blank"><a href="http://www.cisco.com/en/US/products/ps6748/products_installation_guide_chapter09186a008065b0b9.html" target="_blank">http://www.cisco.com/en/US/products/ps6748/products_installation_guide_chapter09186a008065b0b9.html</a></a>
      </dd>
    </dl>
    
    <p>
      2008/12/17追記
    </p>
    
    <dl>
      <dt>
        Understanding Layer 2 Trunk Failover
      </dt>
      
      <dd>
        <a href="http://www.cisco.com/en/US/docs/switches/blades/3020/software/release/12.2_25_sef1/configuration/guide/swethchl.html#wp1346176" target="_blank"><a href="http://www.cisco.com/en/US/docs/switches/blades/3020/software/release/12.2_25_sef1/configuration/guide/swethchl.html#wp1346176" target="_blank">http://www.cisco.com/en/US/docs/switches/blades/3020/software/release/12.2_25_sef1/configuration/guide/swethchl.html#wp1346176</a></a>
      </dd>
    </dl></div> 
    
    <div class="footnote">
      <p class="footnote">
        <a href="#fn1" name="f1" target="_blank">*1</a>：Catalyst2960でも出来たのでCatalystなら出来ると思われる
      </p>
      
      <p class="footnote">
        <a href="#fn2" name="f2" target="_blank">*2</a>：2008/12/17修正:動作するのはupstreamのlinkが変化したときだけだった。downstreamがdownしてもupstreamはdonwしない
      </p>
    </div>