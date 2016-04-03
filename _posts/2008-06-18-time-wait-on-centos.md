---
title: TIME_WAIT を早めに消す on CentOS
date: Wed, 18 Jun 2008 15:00:00 +0000
author: Hiro Fukami
layout: post
permalink: /2008/06/18/time-wait-on-centos/
tumblr_hirofukami_permalink:
  - http://hirofukami.tumblr.com/post/29609177207/time-wait-on-centos
  - http://hirofukami.tumblr.com/post/29609177207/time-wait-on-centos
  - http://hirofukami.tumblr.com/post/29609177207/time-wait-on-centos
  - http://hirofukami.tumblr.com/post/29609177207/time-wait-on-centos
tumblr_hirofukami_id:
  - 29609177207
  - 29609177207
  - 29609177207
  - 29609177207
original_post_id:
  - 373
  - 373
  - 373
  - 373
categories:
  - Tech
tags:
  - Linux
  - Server
---
<div class="section">
  <p>
    異常にwebの表示が遅くなった時の原因が TIME_WAIT の大量発生だった。PHP だと生じやすいなぁ。
  </p>
  
  <p>
    CentOS 4.5 finall にて TIME_WAIT を短くするための方法。
  </p>
  
  <ul>
    <li>
      状況</p> <ul>
        <li>
          web と DB を別サーバでネットワーク経由のデータやりとり
        </li>
        <li>
          DB サーバ側は TIME_WAIT ほとんどなし
        </li>
        <li>
          web(phpで作ったもの) にて TIME_WAIT 多めに残る それでも 150セッションくらいか
        </li>
      </ul>
    </li>
  </ul>
  
  <ul>
    <li>
      確認
    </li>
  </ul>
  
  <blockquote>
    <p>
      # netstat -an | grep -i time_
    </p>
    
    <p>
      (状況確認)
    </p>
    
    <p>
      # netstat -an | grep -i time_ | wc
    </p>
    
    <p>
      (行数として確認)
    </p>
  </blockquote>
  
  <ul>
    <li>
      作業
    </li>
  </ul>
  
  <blockquote>
    <p>
      # cd /proc/sys/net/ipv4/
    </p>
    
    <p>
      # more tcp_tw_recycle
    </p>
    
    <p>
    </p>
    
    <p>
      # echo &#8216;1&#8217; > tcp_tw_recycle
    </p>
  </blockquote>
  
  <p>
    特に再起動などいらず即反映される。
  </p>
  
  <p>
    重めなページを読み込ませつつ
  </p>
  
  <blockquote>
    <p>
      # netstat -an | grep -i time_ | wc
    </p>
  </blockquote>
  
  <p>
    を定期的に見れば数が減っていく速度が速いことが分かる。
  </p>
  
  <ul>
    <li>
      参考サイト</p> <ul>
        <li>
          <a href="http://ohac.pun.jp/blog/2005/200502.html" target="_blank"><a href="http://ohac.pun.jp/blog/2005/200502.html" target="_blank">http://ohac.pun.jp/blog/2005/200502.html</a></a>
        </li>
        <li>
          <a href="http://www.linux.or.jp/JF/JFdocs/Adv-Routing-HOWTO/lartc.kernel.obscure.html" target="_blank"><a href="http://www.linux.or.jp/JF/JFdocs/Adv-Routing-HOWTO/lartc.kernel.obscure.html" target="_blank">http://www.linux.or.jp/JF/JFdocs/Adv-Routing-HOWTO/lartc.kernel.obscure.html</a></a>
        </li>
      </ul>
    </li>
  </ul>
</div>