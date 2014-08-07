---
title: '[Foundry]seting routing table value'
date: Wed, 21 Jul 2004 15:00:00 +0000
author: Hiro Fukami
layout: post
permalink: /2004/07/21/foundry-seting-routing-table-value/
tumblr_hirofukami_permalink:
  - http://hirofukami.tumblr.com/post/29542402902/foundry-seting-routing-table-value
  - http://hirofukami.tumblr.com/post/29542402902/foundry-seting-routing-table-value
  - http://hirofukami.tumblr.com/post/29542402902/foundry-seting-routing-table-value
  - http://hirofukami.tumblr.com/post/29542402902/foundry-seting-routing-table-value
  - http://hirofukami.tumblr.com/post/29542402902/foundry-seting-routing-table-value
tumblr_hirofukami_id:
  - 29542402902
  - 29542402902
  - 29542402902
  - 29542402902
  - 29542402902
original_post_id:
  - 742
  - 742
  - 742
  - 742
  - 742
categories:
  - Tech
tags:
  - Network
  - routing
---
<div class="section">
  <p>
    BigIron で BGP Full Route を持たせたい時、これを入れないと routing table に乗らない経路が出てきてしまうので必須。
  </p>
  
  <blockquote>
    <p>
      :
    </p>
    
    <p>
      system-max ip-route 200000
    </p>
    
    <p>
      :
    </p>
  </blockquote>
  
  <p>
    意味は上限20万経路まで保持。
  </p>
  
  <p>
    設定後は Reboot が必要。
  </p>
  
  <p>
    参考URL
  </p>
  
  <p>
    <a href="http://www.foundrynet.com/services/documentation/srcli/Global_Cfg_cmds.html#36853" target="_blank"><a href="http://www.foundrynet.com/services/documentation/srcli/Global_Cfg_cmds.html#36853" target="_blank">http://www.foundrynet.com/services/documentation/srcli/Global_Cfg_cmds.html#36853</a></a>
  </p>
</div>