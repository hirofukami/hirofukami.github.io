---
title: Amazon RDS の Read Replica 触ってみた
date: Sun, 06 Feb 2011 15:00:00 +0000
author: Hiro Fukami
layout: post
permalink: /2011/02/06/amazon-rds-read-replica/
tumblr_hirofukami_permalink:
  - http://hirofukami.tumblr.com/post/29678362310/amazon-rds-read-replica
  - http://hirofukami.tumblr.com/post/29678362310/amazon-rds-read-replica
  - http://hirofukami.tumblr.com/post/29678362310/amazon-rds-read-replica
tumblr_hirofukami_id:
  - 29678362310
  - 29678362310
  - 29678362310
original_post_id:
  - 182
  - 182
  - 182
categories:
  - Tech
tags:
  - AWS
---
<div class="section">
  <p>
    前から気になっていた Amazon RDS を触ってみた。gumiは自前でインスタンスにMySQLをセットアップせずに<a href="http://aws.amazon.com/jp/solutions/case-studies/gumi/" target="_blank">RDS使っている</a>し、read replicaも出てスケーリングに関してどの程度カバーできるのか興味もあった。やっぱりMySQLの構築や運用は任せられたほうが楽なので。
  </p>
  
  <p>
    <a href="http://f.hatena.ne.jp/d_sea/20110207182011" class="hatena-fotolife" target="_blank"><img src="http://cdn-ak.f.st-hatena.com/images/fotolife/d/d_sea/20110207/20110207182011.png?w=350" alt="f:id:d_sea:20110207182011p:image:w350" title="f:id:d_sea:20110207182011p:image:w350" class="hatena-fotolife" data-recalc-dims="1" /></a>
  </p>
  
  <p>
    作り方はとても簡単。AWS Management Console上ではいくつかのパラメータをいれてクリックしていければ作れる。最初にインスタンスタイプとストレージ容量などを決める必要があるが、これは後から変更も可能。
  </p>
  
  <p>
    ここで特徴的なのは Multi-AZ だと思う。これはおそらくレプリケーションしていて、Masterとは異なるAvailability Zoneにあるインスタンスでホットスタンバイ機が設けられる。万が一、Masterのデータセンターごと落ちちゃった時やメンテナンス時にこのスタンバイ機にフェールオーバーされるらしい。やはりDBはシングルポイントで落ちては困るので、Multi-AZ は付けておきたいが、<a href="http://aws.amazon.com/jp/rds/#pricing" target="_blank">料金も倍かかる</a>ので注意。
  </p>
  
  <p>
    <a href="http://f.hatena.ne.jp/d_sea/20110207182013" class="hatena-fotolife" target="_blank"><img src="http://cdn-ak.f.st-hatena.com/images/fotolife/d/d_sea/20110207/20110207182013.png?w=350" alt="f:id:d_sea:20110207182013p:image:w350" title="f:id:d_sea:20110207182013p:image:w350" class="hatena-fotolife" data-recalc-dims="1" /></a>
  </p>
  
  <p>
    変更画面はこんな感じでいろいろ変えることができる。インスタンスタイプとストレージ容量が途中で変えられるのは、予測しづらい中で運用する際には嬉しいと思う。
  </p>
  
  <p>
    最初のMasterの起動自体は5分くらいかかっている。裏でEBSマウントして、MySQL立ち上げてユーザ設定してとかやっているのだろう。
  </p>
  
  <p>
    <a href="http://f.hatena.ne.jp/d_sea/20110207182014" class="hatena-fotolife" target="_blank"><img src="http://cdn-ak.f.st-hatena.com/images/fotolife/d/d_sea/20110207/20110207182014.png?w=350" alt="f:id:d_sea:20110207182014p:image:w350" title="f:id:d_sea:20110207182014p:image:w350" class="hatena-fotolife" data-recalc-dims="1" /></a>
  </p>
  
  <p>
    Slaveにあたる read replica を作るのもAWS Management Consoleから行える。Masterより設定項目が少ないので、簡単にレプリケーションを張ったSlaveが作れる。read replica は最大5台まで立ち上げられる。
  </p>
  
  <p>
    <a href="http://f.hatena.ne.jp/d_sea/20110207191957" class="hatena-fotolife" target="_blank"><img src="http://cdn-ak.f.st-hatena.com/images/fotolife/d/d_sea/20110207/20110207191957.png?w=830" alt="f:id:d_sea:20110207191957p:image" title="f:id:d_sea:20110207191957p:image" class="hatena-fotolife" data-recalc-dims="1" /></a>
  </p>
  
  <p>
    read replica が立ち上げ中の時はManagement Console上のMasterのstatusはmodifyingになるので、レプリケーションをはるためにMySQLは止まってしまうのだろう。上は新規で立ち上げた read replica のstatus。
  </p>
  
  <p>
    ここから色々探ってみる。
  </p>
  
  <p>
    <a name="seemore"></a>
  </p>
  
  <p>
    ホスト名を引くとec2のホスト名にCNAMEされていた。
  </p>
  
  <pre>

$ host testidentifier.cstgtbgikb7y.us-east-1.rds.amazonaws.com

testidentifier.cstgtbgikb7y.us-east-1.rds.amazonaws.com is an alias for ec2-184-73-82-152.compute-1.amazonaws.com.

ec2-184-73-82-152.compute-1.amazonaws.com has address 184.73.82.152

</pre>
  
  <p>
    内部的にはEC2のインスタンスを起動、EBSマウント、レプリケーションをはる動作を行なっているようだ。起動スクリプトやれないことはないけれども、手間なのでワンクリックだけですむのは非常に楽に構築できる。ちなみに Snapshotもクリックひとつで簡単に取れる。
  </p>
  
  <p>
    Masterにmysqlログインして見てみる。
  </p>
  
  <pre>

dsea-MacBook:~ hironobu$ mysql -u testrep -p**** -h testidentifier.cstgtbgikb7y.us-east-1.rds.amazonaws.com



(snip)



mysql&gt; show databases;

+--------------------+

| Database           |

+--------------------+

| information_schema |

| innodb             |

| mysql              |

| testrds          |

+--------------------+

4 rows in set (0.18 sec)

</pre>
  
  <p>
    testrdsは自分が作ったDB。
  </p>
  
  <pre>

mysql&gt; select user,host from mysql.user;

+--------------+-----------+

| user         | host      |

+--------------+-----------+

| rdsrepladmin | %         |

| testrep      | %         |

| rdsadmin     | localhost |

+--------------+-----------+

3 rows in set (0.19 sec)



mysql&gt; show grants for rdsadmin@localhostG

*************************** 1. row ***************************

Grants for rdsadmin@localhost: GRANT ALL PRIVILEGES ON *.* TO 'rdsadmin'@'localhost' IDENTIFIED BY PASSWORD '*403D726C7C5F4E71D5790A7C0BCEA79CE3BEB17C' WITH GRANT OPTION

1 row in set (0.20 sec)



mysql&gt; show grants for rdsrepladminG

*************************** 1. row ***************************

Grants for rdsrepladmin@%: GRANT REPLICATION SLAVE ON *.* TO 'rdsrepladmin'@'%' IDENTIFIED BY PASSWORD '*132499B6DBF70C249F6262DB94789088E3B76B92'

1 row in set (0.20 sec)



mysql&gt; show grants for testrepG

*************************** 1. row ***************************

Grants for testrep@%: GRANT SELECT, INSERT, UPDATE, DELETE, CREATE, DROP, RELOAD, PROCESS, REFERENCES, INDEX, ALTER, SHOW DATABASES, CREATE TEMPORARY TABLES, LOCK TABLES, EXECUTE, REPLICATION CLIENT, CREATE VIEW, SHOW VIEW, CREATE ROUTINE, ALTER ROUTINE, CREATE USER, EVENT, TRIGGER ON *.* TO 'testrep'@'%' IDENTIFIED BY PASSWORD '*39247272E1DEB1E4B93144EAB61B2F00A752CABC' WITH GRANT OPTION

1 row in set (0.20 sec)





</pre>
  
  <p>
    testrepは自分が起動時に設定したユーザなので、rdsrepladminとrdsadminというのがあらかじめ設定されてるユーザで、rootにあたるのがrdsadmin、read replicaからのレプリケーションを受け付けるためにrdsrepladminがあることが分かる。自分で作ったユーザには管理系のコマンドは実行できないようになっている。
  </p>
  
  <pre>

mysql&gt; show master status;

+----------------------------+----------+--------------+------------------+

| File                       | Position | Binlog_Do_DB | Binlog_Ignore_DB |

+----------------------------+----------+--------------+------------------+

| mysql-bin-changelog.000013 |      106 |              |                  |

+----------------------------+----------+--------------+------------------+

1 row in set (0.18 sec)

</pre>
  
  <p>
    確かにMasterとして動いている。
  </p>
  
  <p>
    次に read replicaにmysqlログインしてみる。
  </p>
  
  <pre>

$ mysql -u testrep -p**** -h replica01testidentifier.cstgtbgikb7y.us-east-1.rds.amazonaws.com



(snip)



mysql&gt; select user,host from mysql.user;

+--------------+-----------+

| user         | host      |

+--------------+-----------+

| rdsrepladmin | %         |

| testrep      | %         |

| rdsadmin     | localhost |

+--------------+-----------+

3 rows in set (0.19 sec)



mysql&gt; show databases;

+--------------------+

| Database           |

+--------------------+

| information_schema |

| innodb             |

| mysql              |

| testopenx          |

+--------------------+

</pre>
  
  <p>
    あたりまえだけどMasterと同じ。
  </p>
  
  <pre>

mysql&gt; show slave statusG

*************************** 1. row ***************************

Slave_IO_State: Waiting for master to send event

Master_Host: testidentifier.cstgtbgikb7y.us-east-1.rds.amazonaws.com

Master_User: rdsrepladmin

Master_Port: 3306

Connect_Retry: 60

Master_Log_File: mysql-bin-changelog.000013

Read_Master_Log_Pos: 106

Relay_Log_File: relaylog.000018

Relay_Log_Pos: 261

Relay_Master_Log_File: mysql-bin-changelog.000013

Slave_IO_Running: Yes

Slave_SQL_Running: Yes

Replicate_Do_DB:

Replicate_Ignore_DB:

Replicate_Do_Table:

Replicate_Ignore_Table:

Replicate_Wild_Do_Table:

Replicate_Wild_Ignore_Table:

Last_Errno: 0

Last_Error:

Skip_Counter: 0

Exec_Master_Log_Pos: 106

Relay_Log_Space: 462

Until_Condition: None

Until_Log_File:

Until_Log_Pos: 0

Master_SSL_Allowed: No

Master_SSL_CA_File:

Master_SSL_CA_Path:

Master_SSL_Cert:

Master_SSL_Cipher:

Master_SSL_Key:

Seconds_Behind_Master: 0

Master_SSL_Verify_Server_Cert: No

Last_IO_Errno: 0

Last_IO_Error:

Last_SQL_Errno: 0

Last_SQL_Error:

1 row in set (0.19 sec)

</pre>
  
  <p>
    Masterのホスト名宛にrdsrepladminユーザでレプリケーションがはれていることが分かる。
  </p>
  
  <p>
    中身を見てしまえば、MySQLのレプリケーションの構成。確かに簡単にレプリケーション環境をクリックだけで作れるのは新しい感覚。
  </p>
  
  <p>
    ここまで作れているのは嬉しいが、やっぱりEC2をベースにしているとはっきり分かるスタンスがユーザビリティに影響している部分がある。せっかく read replica で簡単に増やせても、その把握とクエリを投げる先に加える仕組みはユーザ側で用意しなければならないから、実運用上はサービスを止めてメンテナンスで更新をかけたり、クエリの分散先をスムーズに更新する自動化された仕組みを頑張って用意するのだろう。
  </p>
  
  <p>
    そこら辺を踏まえ、せっかくスケーラブルなベースが出来ているのだから、以下の機能も提供してもらえたら素晴らしくなるのにと思った。
  </p>
  
  <ul>
    <li>
      read replicaのオートスケーリングへの対応</p> <ul>
        <li>
          結局中身はEC2なのだし
        </li>
      </ul>
    </li>
    
    <li>
      増えた read replica用のELB提供 <ul>
        <li>
          Slaveが増やせても複数台それぞれにアクセスするための仕組みはユーザで用意しなくては行けない
        </li>
        <li>
          アプリケーション側で工夫するとかDNSラウンドロビンにするとか。それを一元化するための仕組みは用意してあげたほうがいい
        </li>
      </ul>
    </li>
    
    <li>
      出来ればクエリを見てMaster/read replicaに振る <ul>
        <li>
          アプリケーションから見るとたった1つのホストにアクセスすればいい
        </li>
      </ul>
    </li>
  </ul>
  
  <p>
    上2つはそれほどハードルは高くないと思うので、多分もうAmazonは準備していると思うけど、あとはいつ出てくるかだけかな。
  </p>
</div>