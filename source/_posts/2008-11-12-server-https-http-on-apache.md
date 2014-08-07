---
title: '[server] https できたものを http にリダイレクトさせる on Apache'
date: Wed, 12 Nov 2008 15:00:00 +0000
author: Hiro Fukami
layout: post
permalink: /2008/11/12/server-https-http-on-apache/
tumblr_hirofukami_permalink:
  - http://hirofukami.tumblr.com/post/29609199454/server-https-http-on-apache
  - http://hirofukami.tumblr.com/post/29609199454/server-https-http-on-apache
  - http://hirofukami.tumblr.com/post/29609199454/server-https-http-on-apache
tumblr_hirofukami_id:
  - 29609199454
  - 29609199454
  - 29609199454
original_post_id:
  - 316
  - 316
  - 316
categories:
  - Tech
tags:
  - Server
  - tech
  - web
---
<div class="section">
  <p>
    作業メモ。以下のようなことをしたいときの RedirectMatch の正規表現を忘れてしまいそうなので。
  </p>
  
  <ul>
    <li>
      https でアクセスにきたものを http にリダイレクトさせる
    </li>
    <li>
      その際、ホスト名とそれ以下の URL は保持されること
    </li>
  </ul>
  
  <p>
    例) <a href="https://www.temp.domain.com/temp/temp.html" target="_blank"><a href="https://www.temp.domain.com/temp/temp.html" target="_blank">https://www.temp.domain.com/temp/temp.html</a></a> できたものを <a href="http://www.temp.domain.com/temp/temp.html" target="_blank"><a href="http://www.temp.domain.com/temp/temp.html" target="_blank">http://www.temp.domain.com/temp/temp.html</a></a> へリダイレクトさせる
  </p>
  
  <ul>
    <li>
      前提環境</p> <ul>
        <li>
          複数のホスト名をサービスしているとして、temp-ssl.conf 上、Virtual Host で設定している
        </li>
        <li>
          ここでは仮に temp.domain.com に https できたものを http にリダイレクトする
        </li>
      </ul>
    </li>
  </ul>
  
  <p>
    <ul>
      <li>
        1) /etc/httpd/conf/ or /etc/httpd/conf.d/ にある temp-ssl.conf に以下の記述を追記
      </li>
    </ul>
    
    <pre>

&lt;VirtualHost *:443&gt;

ServerName temp.domain.com

ServerAlias temp.domain.com

RedirectMatch ^(.*)$ <a href="http://temp.domain.com%241" target="_blank"><a href="http://temp.domain.com" target="_blank">http://temp.domain.com</a>$1</a>

</pre>
    
    <ul>
      <li>
        2) Apache の再起動する # /etc/rc.d/init.d/httpd restart</p> <ul>
          <li>
            ここで httpd graceful するとゾンビプロセス(ps aux での status Z)がでてしまうケースがあった
          </li>
        </ul>
      </li>
    </ul>
    
    <p>
      正規表現はなかなか覚えられないので、なんかのときに役立てば。
    </p></div>