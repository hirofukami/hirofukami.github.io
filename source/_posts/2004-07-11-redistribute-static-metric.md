---
title: Redistribute static metric
date: Sun, 11 Jul 2004 15:00:00 +0000
author: Hiro Fukami
layout: post
permalink: /2004/07/11/redistribute-static-metric/
tumblr_hirofukami_permalink:
  - http://hirofukami.tumblr.com/post/29542402654/redistribute-static-metric
  - http://hirofukami.tumblr.com/post/29542402654/redistribute-static-metric
  - http://hirofukami.tumblr.com/post/29542402654/redistribute-static-metric
  - http://hirofukami.tumblr.com/post/29542402654/redistribute-static-metric
  - http://hirofukami.tumblr.com/post/29542402654/redistribute-static-metric
tumblr_hirofukami_id:
  - 29542402654
  - 29542402654
  - 29542402654
  - 29542402654
  - 29542402654
original_post_id:
  - 743
  - 743
  - 743
  - 743
  - 743
categories:
  - Tech
tags:
  - Network
---
<div class="section">
  <p>
    Cisco と Extreme での redistribute static metric の足し方(type 1)が異なることに気づく。
  </p>
  
  <p>
    上流から見た場合
  </p>
  
  <ul>
    <li>
      Cisco</p> <ul>
        <li>
          core interface cost + redistribute metric
        </li>
      </ul>
    </li>
    
    <li>
      Extreme <ul>
        <li>
          core interface cost + redistribute metric + edge interface cost
        </li>
      </ul>
    </li>
  </ul>
  
  <p>
    確か、Juniper も Ciscoと同じだったはず。
  </p>
  
  <p>
    でもホントにメーカの仕様 issue？
  </p>
</div>