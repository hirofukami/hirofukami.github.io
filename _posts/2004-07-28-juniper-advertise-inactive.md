---
title: '[Juniper]advertise-inactive'
date: Wed, 28 Jul 2004 15:00:00 +0000
author: Hiro Fukami
layout: post
permalink: /2004/07/28/juniper-advertise-inactive/
tumblr_hirofukami_permalink:
  - http://hirofukami.tumblr.com/post/29542403358/juniper-advertise-inactive
  - http://hirofukami.tumblr.com/post/29542403358/juniper-advertise-inactive
  - http://hirofukami.tumblr.com/post/29542403358/juniper-advertise-inactive
  - http://hirofukami.tumblr.com/post/29542403358/juniper-advertise-inactive
  - http://hirofukami.tumblr.com/post/29542403358/juniper-advertise-inactive
tumblr_hirofukami_id:
  - 29542403358
  - 29542403358
  - 29542403358
  - 29542403358
  - 29542403358
original_post_id:
  - 740
  - 740
  - 740
  - 740
  - 740
categories:
  - Tech
tags:
  - Juniper
  - Network
---
<div class="section">
  <p>
    階層的には
  </p>
  
  <blockquote>
    <p>
      bgp {
    </p>
    
    <p>
      advertise-inactive;
    </p>
    
    <p>
      &#160;:
    </p>
  </blockquote>
  
  <p>
    BGPでもIGPでも同じ prefix を広報している場合。
  </p>
  
  <p>
    # 顧客割り当てblockがそのままBGPのnetworkで設定されるなどの場合
  </p>
  
  <p>
    Juniper的 administrative distance ではOSPFが勝つのでBGPには乗らない。
  </p>
  
  <p>
    BGPのエントリーはBestではないけど、乗せてあげるという意味。
  </p>
  
  <p>
    eBGPで広報してるつもりが出来ていないことになるのでこれは必須。
  </p>
  
  <p>
    <p>
      参考URL
    </p>
    
    <p>
      <a href="http://www.juniper.net/techpubs/software/junos/junos62/swconfig62-policy/html/policy-overview-framework4.html#1021027" target="_blank"><a href="http://www.juniper.net/techpubs/software/junos/junos62/swconfig62-policy/html/policy-overview-framework4.html#1021027" target="_blank">http://www.juniper.net/techpubs/software/junos/junos62/swconfig62-policy/html/policy-overview-framework4.html#1021027</a></a>
    </p></div>