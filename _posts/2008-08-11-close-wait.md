---
title: CLOSE_WAITのセッションを早く消す方法
date: Mon, 11 Aug 2008 15:00:00 +0000
author: Hiro Fukami
layout: post
permalink: /2008/08/11/close-wait/
tumblr_hirofukami_permalink:
  - http://hirofukami.tumblr.com/post/29609187338/close-wait
  - http://hirofukami.tumblr.com/post/29609187338/close-wait
  - http://hirofukami.tumblr.com/post/29609187338/close-wait
tumblr_hirofukami_id:
  - 29609187338
  - 29609187338
  - 29609187338
original_post_id:
  - 346
  - 346
  - 346
categories:
  - Tech
tags:
  - Server
---
<div class="section">
  <p>
    原因はおそらくwebサーバ～DBサーバ間のセッションがネットワーク的に不安定だったからだと思うけど、DBサーバで netstat -an すると 大量の CLOSE_WAIT のセッションが残ってしまって、MySQL の最大コネクション数にまで達してしまってwebの表示が出来なかった。
  </p>
  
  <p>
    CLOSE_WAIT をいきなり消すことは出来ないので、早めに消えるようにしたいときは
  </p>
  
  <dl>
    <dt>
      tcp_keepalive_time
    </dt>
    
    <dd>
      TCPセッションが確立されて検査が実施されるまでの時間<br />時間になると tcp_keepalive_intvl と tcp_keepalive_probes の値にしたがって検査を実施する
    </dd>
    
    <dt>
      tcp_keepalive_intvl
    </dt>
    
    <dd>
      次の検査を実行するときの待ち時間
    </dd>
    
    <dt>
      tcp_keepalive_probes
    </dt>
    
    <dd>
      検査の回数
    </dd>
  </dl>
  
  <p>
    のそれぞれの値を小さくしてあげればよい。
  </p>
  
  <p>
    変更方法は vi で :wq すると error がでておこられるので、echo を使う。反映に再起動などはいらない。
  </p>
  
  <blockquote>
    <p>
      # echo 10 > /proc/sys/net/ipv4/tcp_keepalive_time
    </p>
    
    <p>
      # echo 2 > /proc/sys/net/ipv4/tcp_keepalive_probes
    </p>
    
    <p>
      # echo 3 > /proc/sys/net/ipv4/tcp_keepalive_intvl
    </p>
  </blockquote>
  
  <p>
    デフォルト値と変更後の値と CLOSE_WAIT が切れるまでの時間を表にする
  </p>
  
  <table>
    <tr>
      <th>
        項目
      </th>
      
      <th>
        デフォルト
      </th>
      
      <th>
        変更後
      </th>
    </tr>
    
    <tr>
      <td>
        tcp_keepalive_time
      </td>
      
      <td>
        7200(sec)
      </td>
      
      <td>
        10(sec)
      </td>
    </tr>
    
    <tr>
      <td>
        tcp_keepalive_probes
      </td>
      
      <td>
        9(回)
      </td>
      
      <td>
        2(回)
      </td>
    </tr>
    
    <tr>
      <td>
        tcp_keepalive_intvl
      </td>
      
      <td>
        75(sec)
      </td>
      
      <td>
        3(sec)
      </td>
    </tr>
    
    <tr>
      <td>
        TCPセッションが確立して最短で落ちるまでの時間
      </td>
      
      <td>
        7200+9*75=7875(sec) (2時間10分くらい)
      </td>
      
      <td>
        10+2*3=16(sec)
      </td>
    </tr>
  </table>
  
  <p>
    緊急時だったのでこのくらいにして、期待通りに CLOSE_WAIT がどんどん消えていった。
  </p>
  
  <p>
    今でもこの設定のまま様子見だけど、TCPセッションが確立されているものを切ってしまうわけではないので、問題はないのでは？と勝手に思っている
  </p>
  
  <p>
    上記は一時的な反映をしたい時の実施方法で、OS再起動で消えてしまう。
  </p>
  
  <p>
    消えないためには、
  </p>
  
  <blockquote>
    <p>
      # vi /etc/sysctl.conf
    </p>
    
    <p>
      net.ipv4.tcp_keepalive_time = 10
    </p>
    
    <p>
      net.ipv4.tcp_keepalive_probes = 2
    </p>
    
    <p>
      net.ipv4.tcp_keepalive_intvl = 3
    </p>
    
    <p>
      を追記
    </p>
    
    <p>
      # sysctl -w
    </p>
    
    <p>
      変更を反映
    </p>
    
    <p>
      # sysctl -p
    </p>
    
    <p>
      確認
    </p>
  </blockquote>
  
  <p>
    のようにする
  </p>
  
  <ul>
    <li>
      参考サイト</p> <ul>
        <li>
          <a href="http://d.hatena.ne.jp/koseki2/20070331/closewait" target="_blank"><a href="http://d.hatena.ne.jp/koseki2/20070331/closewait" target="_blank">http://d.hatena.ne.jp/koseki2/20070331/closewait</a></a>
        </li>
        <li>
          <a href="http://unix-study.com/weblog+index.date+200704.htm" target="_blank"><a href="http://unix-study.com/weblog+index.date+200704.htm" target="_blank">http://unix-study.com/weblog+index.date+200704.htm</a></a>
        </li>
      </ul>
    </li>
  </ul>
</div>