---
title: SSL証明書更新 on Apache + OpenSSL 手順メモ
date: Wed, 10 Dec 2008 15:00:00 +0000
author: Hiro Fukami
layout: post
permalink: /2008/12/10/ssl-on-apache-openssl/
tumblr_hirofukami_permalink:
  - http://hirofukami.tumblr.com/post/29609205161/ssl-on-apache-openssl
  - http://hirofukami.tumblr.com/post/29609205161/ssl-on-apache-openssl
  - http://hirofukami.tumblr.com/post/29609205161/ssl-on-apache-openssl
tumblr_hirofukami_id:
  - 29609205161
  - 29609205161
  - 29609205161
original_post_id:
  - 302
  - 302
  - 302
categories:
  - Tech
tags:
  - apache
  - openssl
  - Server
  - SSL
---
<div class="section">
  <p>
    更新頻度が1年に1回だからその都度忘れてしまうのでメモ。
  </p>
  
  <h5>
    1) 秘密鍵を生成
  </h5>
  
  <p>
    openssl コマンドで生成<br />擬似乱数情報の生成 (md5ダイジェスト値を擬似乱数をして使用)
  </p>
  
  <blockquote>
    <p>
      # ./openssl md5 * > rand.dat
    </p>
  </blockquote>
  
  <p>
    擬似乱数ファイル(ここでは rand.dat)から秘密鍵を生成<br />トリプルDESを使用、2048bitの秘密鍵(ここでは 2009key.pem を作成
  </p>
  
  <blockquote>
    <p>
      # openssl genrsa -rand rand.dat -des3&#160;2048 > 2009key.pem
    </p>
  </blockquote>
  
  <p>
    パスフレーズを入力するので、忘れないように。
  </p>
  
  <h5>
    2) 秘密鍵からCSRを生成
  </h5>
  
  <p>
    作成した秘密鍵(2009key.pem)からCSR(2009csr.pem)を生成する
  </p>
  
  <blockquote>
    <p>
      # openssl req -new -key 2009key.pem -out 2009csr.pem
    </p>
  </blockquote>
  
  <p>
    この時に秘密鍵のパスフレーズを求められる<br />以下、証明書に登録される情報(ディスティングイッシュネーム情報)を入れる<br />更新前の証明書と同じ内容にする必要があるので、更新前の証明書を控えておく
  </p>
  
  <ul>
    <li>
      Country(国名): JP
    </li>
    <li>
      State(都道府県名): Tokyo
    </li>
    <li>
      Locality(市区町村名): Shibuya-ku
    </li>
    <li>
      Organizational Name(組織名): ShakeSoul,Inc.
    </li>
    <li>
      Organizational Unit(部門名): System1
    </li>
    <li>
      Common Name(コモンネーム): <a href="http://www.shakesoul.net" target="_blank">www.shakesoul.net</a>
    </li>
    <li>
      以下は入力せずに Enter キーを押して進める <ul>
        <li>
          Email Address []:
        </li>
        <li>
          A challenge password []:
        </li>
        <li>
          An optional company name []:
        </li>
      </ul>
    </li>
  </ul>
  
  <p>
    CSRが生成される<br />CSRの内容を発行局へ提出
  </p>
  
  <h5>
    3) 各ファイルのパーミッション変更と秘密鍵のパスワード削除
  </h5>
  
  <ul>
    <li>
      発行局から送られてきた証明書をコピペするなどしてファイル化する</p> <ul>
        <li>
          この場合 2009cert.pem とする
        </li>
      </ul>
    </li>
    
    <li>
      中間CA証明書が必要な場合はコピペするなどしてファイル化する <ul>
        <li>
          この場合 CA-cert.pem とする
        </li>
      </ul>
    </li>
    
    <li>
      各ファイルのパーミッションを以下のようにする
    </li>
  </ul>
  
  <blockquote>
    <ul>
      <li>
        rw&#8212;&#8212;&#8212;- 1 root root 1854&#160;12月 3&#160;11:32 2009cert.pem
      </li>
    </ul>
    
    <ul>
      <li>
        rw&#8212;&#8212;&#8212;- 1 root root 1696&#160;12月 13 2007 CA-cert.pem
      </li>
    </ul>
    
    <ul>
      <li>
        rw&#8212;&#8212;&#8212;- 1 root root 887&#160;12月 3&#160;11:24 2009key.pem
      </li>
    </ul>
  </blockquote>
  
  <ul>
    <li>
      秘密鍵のパスフレーズを削除する 起動時にパスワード入力を求められないようにするため以下を実行
    </li>
  </ul>
  
  <blockquote>
    <p>
      # openssl rsa -in 2009key.pem -out 2009key.pem
    </p>
  </blockquote>
  
  <h5>
    4) 証明書のインストール
  </h5>
  
  <ul>
    <li>
      事前に作成した秘密鍵と証明書を Apache の config ファイルで指定する</p> <ul>
        <li>
          例えば、以下にファイルがあるとすると</p> <ul>
            <li>
              証明書ファイル: /usr/local/ssl/certs/2009cert.pem
            </li>
            <li>
              秘密鍵ファイル: /usr/local/ssl/private/2009key.pem
            </li>
            <li>
              中間CA証明書ファイル: /usr/local/ssl/certs/CA-cert.pem
            </li>
          </ul>
        </li>
        
        <li>
          &#8230;/conf.d/ssl.conf の中を以下のように書き換える
        </li>
      </ul>
    </li>
  </ul>
  
  <blockquote>
    <p>
      SSLCertificateFile /usr/local/ssl/certs/2009cert.pem
    </p>
    
    <p>
      SSLCertificateKeyFile /usr/local/ssl/private/2009key.pem
    </p>
    
    <p>
      SSLCertificateChainFile /usr/local/ssl/certs/CA-cert.pem
    </p>
  </blockquote>
  
  <h5>
    5) Apache 再起動
  </h5>
  
  <blockquote>
    <p>
      # /etc/rc.d/init.d/httpd restart
    </p>
  </blockquote>
  
  <p>
    tail /var/log/httpd/ssl_error_log がでてなければOK
  </p>
</div>