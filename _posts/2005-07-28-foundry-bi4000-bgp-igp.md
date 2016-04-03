---
title: '[Foundry] BI4000 BGP->IGP'
date: Thu, 28 Jul 2005 15:00:00 +0000
author: Hiro Fukami
layout: post
permalink: /2005/07/28/foundry-bi4000-bgp-igp/
tumblr_hirofukami_permalink:
  - http://hirofukami.tumblr.com/post/29542426373/foundry-bi4000-bgp-igp
  - http://hirofukami.tumblr.com/post/29542426373/foundry-bi4000-bgp-igp
  - http://hirofukami.tumblr.com/post/29542426373/foundry-bi4000-bgp-igp
  - http://hirofukami.tumblr.com/post/29542426373/foundry-bi4000-bgp-igp
tumblr_hirofukami_id:
  - 29542426373
  - 29542426373
  - 29542426373
  - 29542426373
original_post_id:
  - 660
  - 660
  - 660
  - 660
categories:
  - Tech
tags:
  - Foundry
  - Network
  - Router
---
<div class="section">
  <p>
    Cisco/Juniper では BGP で受けた経路情報の Nexthop を IGP で解決するとき、IGP的Nexthop が複数あっても &#8220;show ip route&#8221; で BGP からの経路情報には Nexthop が全てエントリーされる。
  </p>
  
  <p>
    例えば BGP の Nexthop が loopback で、そこまでは OSPF の Equal Cost Multi Path(ECMP) で 2path 見えてたとするとき、ちゃんとルーティングテーブルの Nexthop は2つ見えてる。
  </p>
  
  <p>
    しかし、なぜか Foundry BI4000 だと、BGP からの経路情報は Nexthop がデフォルトひとつまでしかエントリーできない。2つ Nexthop をエントリーしたいときは &#8220;BGP multi path 2&#8221; としなければ載らない。
  </p>
  
  <p>
    この挙動は多分正しくない。BGP の経路情報を IGP で正しく解決できていないことになる。BGP -> IGP が出来ていない。
  </p>
  
  <p>
    &#8220;BGP は常に Best な経路しか乗せない&#8221; という意味が間違って反映されてる。
  </p>
  
  <p>
    この挙動に気づくのに結構時間かかったなぁ。。。
  </p>
</div>