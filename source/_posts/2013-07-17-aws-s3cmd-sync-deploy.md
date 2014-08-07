---
title: AWS s3cmd を使ってS3から複数EC2インスタンスへ簡単デプロイ環境を作る
date: Wed, 17 Jul 2013 17:39:26 +0000
author: Hiro Fukami
layout: post
permalink: /2013/07/17/aws-s3cmd-sync-deploy/
geo_public:
  - 0
  - 0
categories:
  - Tech
tags:
  - AWS
  - deploy
  - develop
  - Infra
  - s3cmd
  - sync
---
基本的に複数台の EC2 インスタンスある場合にファイルをデプロイする時は、手動なら SCP で1台ずつやるのだろうけど、楽をするなら Chef を使って環境を作るとかになると思う。だけど、オートスケーリングをさせている場合、起動しているインスタンスは常に変わってしまう懸念があるので、実際そこまで作りこむのは大変。

そこで、s3cmd sync を使うと簡単デプロイ環境ができると思って触ってみた。  
インストールや基本的な動かし方は下記参考URLを見ながら、それ以外の情報も得られたので載せておく。

参考URL

*   http://blog.serverworks.co.jp/tech/2012/06/27/nginx-01/
*   http://memocra.blogspot.jp/2013/02/s3s3cmd-syncinotifys3cmd-sync.html
*   http://understeer.hatenablog.com/entry/2012/07/23/124402
*   http://recipe.kc-cloud.jp/archives/1059
*   http://blog.serverworks.co.jp/tech/2013/04/23/ec-cube-on-aws-s3/
*   http://understeer.hatenablog.com/entry/2012/07/23/124402

## インストール

<pre class="brush: bash; title: ; notranslate" title="">$ sudo yum -y --enablerepo epel install s3cmd
</pre>

AWS アカウントの Access Key と Secret Key の情報を設定する。HTTPSを使うか、HTTP Proxyを使うかとかあるが適宜入力する。

<pre class="brush: bash; title: ; notranslate" title="">$ s3cmd --configure

Enter new values or accept defaults in brackets with Enter.
Refer to user manual for detailed description of all options.

Access key and Secret key are your identifiers for Amazon S3
Access Key: [PUT YOUR ACCESS KEY]
Secret Key: [PUT YOUR SECRET KEY]

Encryption password is used to protect your files from reading
by unauthorized persons while in transfer to S3
Encryption password: [PUT YOUR PASSWORD]
Path to GPG program [/usr/bin/gpg]:

When using secure HTTPS protocol all communication with Amazon S3
servers is protected from 3rd party eavesdropping. This method is
slower than plain HTTP and can't be used if you're behind a proxy
Use HTTPS protocol [No]:

On some networks all internet access must go through a HTTP proxy.
Try setting it here if you can't conect to S3 directly
HTTP Proxy server name:

New settings:
  Access Key: [PUT YOUR ACCESS KEY]
  Secret Key: [PUT YOUR SECRET KEY]
  Encryption password: [PUT YOUR PASSWORD]
  Path to GPG program: /usr/bin/gpg
  Use HTTPS protocol: False
  HTTP Proxy server name:
  HTTP Proxy server port: 0

Test access with supplied credentials? [Y/n] y
Please wait...
Success. Your access key and secret key worked fine <img src='http://i2.wp.com/hirofukami.com/wp-includes/images/smilies/icon_smile.gif?w=830' alt=':-)' class='wp-smiley' data-recalc-dims="1" /> 

Now verifying that encryption works...
Success. Encryption and decryption worked fine <img src='http://i2.wp.com/hirofukami.com/wp-includes/images/smilies/icon_smile.gif?w=830' alt=':-)' class='wp-smiley' data-recalc-dims="1" /> 

Save settings? [y/N] y
   * Configuration saved to '/home/ec2-user/.s3cfg'
</pre>

一応、設定ファルのパスをメモっておく。

## sync してみる

<pre class="brush: bash; title: ; notranslate" title="">$ sudo s3cmd sync s3://deesea-bucket/test-dir/ ./test-s3fs/
(snip)
Done. Downloaded 20912437 bytes in 54.9 seconds, 372.27 kB/s

$ du -h --max-depth=1 test-s3fs/
18M     test-s3fs/wordpress
23M     test-s3fs/
</pre>

23MBで55secくらいかかった。

作成されたディレクトリファイルは実行したユーザの所有権が付与される。

<pre class="brush: bash; title: ; notranslate" title="">$ ll plugins
合計 20
drwxr-xr-x 2 root root 4096  7月  2 06:40 2013 testdir01
-rwxr-xr-x 1 root root 2262  7月  2 06:16 2013 testfile01.txt
</pre>

パーミッションは 755。

テストの時は &#8211;dry-run つけると実際のファイルは更新せずに実行結果の出力をしてくれる。

sync コマンドの注意点は S3 にあってインスタンスにないファイル・ディレクトリは追加してくれるが、S3 になくてインスタンスにあるものは削除してくれない。sync と言いつつ実は追加・更新のみで削除はしてくれない。これを解消するために &#8211;delete-removed がある。

## &#8211;delete-removed を試す

<pre class="brush: plain; title: ; notranslate" title="">$ sudo s3cmd --delete-removed sync s3://deesea-bucket/test-dir/ ./test-s3fs/
</pre>

ただこれも注意点があって、ファイルは削除するがディレクトリは残ったままになる。ディレクトリ内のファイルはなくなるがディレクトリまでは削除してくれるない。もしプログラム的に「ディレクトリがあったら」という条件式を書く場合は不具合が起きるので「ディレクトリ内のファイルが1つでもあれば」というようにすると良いだろう。

## &#8211;exclude を試す

ある特定のディレクトリ・ファイルを sync の対象から外したい時は &#8211;exclude を付ける。  
特定のファイル名なら &#8211;exclude=&#8221;testfile01.txt&#8221; とすればよいのだけど、特定のディレクトリ配下のファイルすべて除外するという指定する時が結構やっかいだった。  
結局、&#8211;exclude=&#8221;\*exclude-dir/\*&#8221; で動いた。

<pre class="brush: bash; title: ; notranslate" title="">$ sudo s3cmd --delete-removed --exclude=&quot;*exclude-dir/*&quot; sync s3://deesea-bucket/test-dir/ ./test-s3fs/
</pre>

これを継続的に回す cron に記述すれば定期的に処理できる。

<pre class="brush: bash; title: ; notranslate" title="">$ sudo crontab -e

*/2 * * * * s3cmd --delete-removed sync s3://deesea-bucket/test-dir/ /usr/share/nginx/html/test-s3fs/
</pre>

2分間隔で処理する記述。

AMIに仕込んでおけば、インスタンスが変わっても気にすることなくデプロイできる。うまく使えばとても楽にデプロイ環境が作れるので、オススメです。