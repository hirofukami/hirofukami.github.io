---
title: '[GR4000] SNMP Response source address'
date: Mon, 14 Nov 2005 15:00:00 +0000
author: Hiro Fukami
layout: post
permalink: /2005/11/14/gr4000-snmp-response-source-address/
tumblr_hirofukami_permalink:
  - http://hirofukami.tumblr.com/post/29542435306/gr4000-snmp-response-source-address
  - http://hirofukami.tumblr.com/post/29542435306/gr4000-snmp-response-source-address
  - http://hirofukami.tumblr.com/post/29542435306/gr4000-snmp-response-source-address
  - http://hirofukami.tumblr.com/post/29542435306/gr4000-snmp-response-source-address
  - http://hirofukami.tumblr.com/post/29542435306/gr4000-snmp-response-source-address
tumblr_hirofukami_id:
  - 29542435306
  - 29542435306
  - 29542435306
  - 29542435306
  - 29542435306
original_post_id:
  - 622
  - 622
  - 622
  - 622
  - 622
categories:
  - Tech
tags:
  - GR4000
  - Network
  - Router
  - snmp
---
<div class="section">
  <p>
    loopback address 宛てに SNMP get をした時にどうも F/W に引っかかると思ったら、応答時の source address が interface address で返しているみたい。
  </p>
  
  <p>
    これは仕様で コマンドでは対応できない。また今後の開発予定もないとのこと。
  </p>
  
  <p>
    ただ、trap 送出時の source address は loopbackを設定しておくとちゃんと loopback address で返すそうな。# 当たり前か。。。
  </p>
  
  <p>
    ちなみに、Cisco/Juniper だと loopback 宛てに聞きに行ったら返りも loopback address で返してくれる。
  </p>
  
  <p>
    どうも細かい部分でいまいちなんだよなぁ～ ＞ GR
  </p>
</div>