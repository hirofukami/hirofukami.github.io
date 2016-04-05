---
title: Amazon EBS と boto を使って自動バックアップ環境を構築する
date: Sun, 25 Oct 2009 15:00:00 +0000
author: Hiro Fukami
layout: post
permalink: /2009/10/25/amazon-ebs-boto/
tumblr_hirofukami_permalink:
  - http://hirofukami.tumblr.com/post/29678346287/amazon-ebs-boto
  - http://hirofukami.tumblr.com/post/29678346287/amazon-ebs-boto
  - http://hirofukami.tumblr.com/post/29678346287/amazon-ebs-boto
tumblr_hirofukami_id:
  - 29678346287
  - 29678346287
  - 29678346287
original_post_id:
  - 211
  - 211
  - 211
categories:
  - Tech
tags:
  - AWS
  - backup
  - boto
  - Server
  - tips
---
<div class="section">
  <p>
    Amazon EBS の特徴はマウントしたボリュームに対するスナップショットが取れることだろう。boto を使って簡単にバックアップ環境を以前に作業したメモを残しておく。
  </p>
  
  <p>
    boto は python のライブラリで、Amazon EC2 のインスタンス上から AWS の API を操作できる。boto の機能は多種であるが、今回は EBS の API を操作して、毎日1回スナップショットをとって5世代分保存する設定をしてみる。
  </p>
  
  <ul>
    <li>
      botoのページ: <a href="http://code.google.com/p/boto/" target="_blank"><a href="http://code.google.com/p/boto/" target="_blank">http://code.google.com/p/boto/</a></a>
    </li>
    <li>
      参考にしたURL <ul>
        <li>
          <a href="http://2606-36j.blogspot.com/2009/03/amazon-ebs-snapshot.html" target="_blank"><a href="http://2606-36j.blogspot.com/2009/03/amazon-ebs-snapshot.html" target="_blank">http://2606-36j.blogspot.com/2009/03/amazon-ebs-snapshot.html</a></a>
        </li>
        <li>
          <a href="http://emutyworks.com/?Amazon%20EC2%2FEBS%E3%81%AESnapshot%E3%82%92%E5%AE%9A%E6%9C%9F%E7%9A%84%E3%81%AB%E5%8F%96%E3%82%8B" target="_blank"><a href="http://emutyworks.com/?Amazon%20EC2%2FEBS%E3%81%AESnapshot%E3%82%92%E5%AE%9A%E6%9C%9F%E7%9A%84%E3%81%AB%E5%8F%96%E3%82%8B" target="_blank">http://emutyworks.com/?Amazon%20EC2%2FEBS%E3%81%AESnapshot%E3%82%92%E5%AE%9A%E6%9C%9F%E7%9A%84%E3%81%AB%E5%8F%96%E3%82%8B</a></a>
        </li>
      </ul>
    </li>
  </ul>
  
  <h4>
    構築手順
  </h4>
  
  <p>
    1. boto インストール
  </p>
  
  <ul>
    <ul>
      <li>
        pythonで動作するのでインストール
      </li>
    </ul>
  </ul>
  
  <pre>

# yum -y install python-devel

</pre>
  
  <ul>
    <ul>
      <li>
        boto ページより最新版をダウンロード
      </li>
    </ul>
  </ul>
  
  <pre>

# wget <a href="http://boto.googlecode.com/files/boto-1.8d.tar.gz" target="_blank"><a href="http://boto.googlecode.com/files/boto-1.8d.tar.gz" target="_blank">http://boto.googlecode.com/files/boto-1.8d.tar.gz</a></a>

# tar zvxf boto-1.8d.tar.gz

# cd boto-1.8d

# python setup.py install

</pre>
  
  <p>
    2. 自動処理用スクリプトの作成
  </p>
  
  <ul>
    <ul>
      <li>
        ファイルパスは &#8220;/home/snapshot.py&#8221; とした
      </li>
      <li>
        aws_access_key_idとaws_secret_access_key の部分を書き換えるだけ。仮に aws_access_key_id は ACCESS KEY、aws_secret_access_key は SECRET KEY としてる。各環境によるそれぞれの値に置き換えてください。
      </li>
      <li>
        作ったSnapshotのdescriptionも書けるのでお好みに変えてください。
      </li>
    </ul>
  </ul>
  
  <pre class="syntax-highlight">

<span class="synComment">#!/usr/bin/python </span>

<span class="synPreProc">import</span> sys

<span class="synPreProc">from</span> boto.ec2.connection <span class="synPreProc">import</span> EC2Connection

<span class="synStatement">if</span>(len(sys.argv) != 3):

<span class="synStatement">print</span> "<span class="synConstant">Usage: snapshot.py &lt;num&gt; &lt;volume-id&gt;</span>"

sys.exit()

conn = EC2Connection('<span class="synConstant">ACCESS KEY</span>','<span class="synConstant">SECRET KEY</span>')

conn.create_snapshot(sys.argv[2], description='<span class="synConstant">This is Backup Snapshot by snapshot.py</span>')

snapshot = {}

<span class="synStatement">for</span> x <span class="synStatement">in</span> conn.get_all_snapshots():

<span class="synStatement">if</span>(x.volume_id == sys.argv[2]):

tmp = {x.id:x.start_time}

snapshot.update(tmp)

snapshot = sorted(snapshot.items(), key=<span class="synStatement">lambda</span> (k, v): (v, k), reverse=True)

<span class="synStatement">for</span> i <span class="synStatement">in</span> range(int(sys.argv[1]), len(snapshot)):

conn.delete_snapshot(snapshot[i][0])

</pre>
  
  <p>
    3. cron に仕込む
  </p>
  
  <ul>
    <ul>
      <li>
        EBS の vol ID は仮で &#8220;vol-12345678&#8221; としている。それぞれの環境に置き換えてください
      </li>
      <li>
        &#8220;# crontab -e&#8221; して以下の内容を記述
      </li>
    </ul>
  </ul>
  
  <pre>

45 04 * * * /home/snapshot.py 5 vol-12345678

</pre>
  
  <ul>
    <ul>
      <li>
        記述方法は、&#8221; [分] [時] [日] [月] [曜日] [filepath] [残したい世代数] [EBSのVolume ID] &#8221; 例は毎日AM4:45に処理させる記述内容
      </li>
    </ul>
  </ul>
  
  <h4>
    動作確認
  </h4>
  
  <p>
    動作確認用として、
  </p>
  
  <pre>

45 * * * * /home/snapshot.py 5 vol-12345678

</pre>
  
  <p>
    として、分の部分を変化させながら動作確認。5世代分まで保存できた。確認してみる。
  </p>
  
  <ul>
    <ul>
      <li>
        -K で private key ファイルを指定している。仮に &#8220;pk-xxxxxxxx.pem&#8221; としているので、それぞれの環境に合わせて置き換えてください
      </li>
      <li>
        -C で certificate file を指定している。仮に &#8220;cert-xxxxxxxx.pem&#8221; としているので、それぞれの環境に合わせて置き換えてください
      </li>
    </ul>
  </ul>
  
  <pre>

$ ec2-describe-snapshots -K pk-xxxxxxxx.pem -C cert-xxxxxxxx.pem

SNAPSHOT    snap-42cc692b    vol-12345678

completed    2009-08-12T03:03:01+0000    100%

SNAPSHOT    snap-e9cc6980    vol-12345678

completed    2009-08-12T03:10:02+0000    100%

SNAPSHOT    snap-3ec96c57    vol-12345678

completed    2009-08-12T03:55:01+0000    100%

SNAPSHOT    snap-c6c86daf    vol-12345678

completed    2009-08-12T04:05:01+0000    100%

SNAPSHOT    snap-27cb6e4e    vol-12345678

completed    2009-08-12T04:12:01+0000    100%

</pre>
  
  <p>
    さらにcronを回しても6世代分にはならず、最も古いものを削除して5世代分を保持してくれた。
  </p>
  
  <pre>

$ ec2-describe-snapshots -K pk-xxxxxxxx.pem -C cert-xxxxxxxx.pem

SNAPSHOT    snap-e9cc6980    vol-12345678

completed    2009-08-12T03:10:02+0000    100%

SNAPSHOT    snap-3ec96c57    vol-12345678

completed    2009-08-12T03:55:01+0000    100%

SNAPSHOT    snap-c6c86daf    vol-12345678

completed    2009-08-12T04:05:01+0000    100%

SNAPSHOT    snap-27cb6e4e    vol-12345678

completed    2009-08-12T04:12:01+0000    100%

SNAPSHOT    snap-c4cb6ead    vol-12345678

completed    2009-08-12T04:16:02+0000    100%

</pre>
  
  <p>
    最も古いタイムスタンプが消えて、新しいものが増えていることが分かる。
  </p>
  
  <p>
    crontab を
  </p>
  
  <pre>

45 04 * * * /home/snapshot.py 5 vol-581cf731

</pre>
  
  <p>
    に戻して確認終了。
  </p>
  
  <p>
    よく S3 にバックアップと言うが、まずは大事なファイルは EBS で作ってマウントしたボリューム上に置いて、この仕組みにのせればバックアップ環境としては十分だろう。
  </p>
  
  <p>
    シェルを駆使しして書いた頃が遠い昔のようだ。。。従来のような手間が AWS のサービスをうまく使うことで簡単に終わってしまう。この新しい感覚と手法は実践的に使ってみないと伝わらないんだろうな。
  </p>
</div>