---
title: AWS s3fsを使って複数EC2インスタンスに共有ストレージを作る
date: Sat, 08 Jun 2013 12:25:56 +0000
author: Hiro Fukami
layout: post
permalink: /2013/06/08/aws-s3fs-multi-ec2/
categories:
  - Tech
tags:
  - auto scaling
  - AWS
  - develop
  - Infra
  - s3fs
---
最近クラウドコンピューティングEXPOに行ったりしてやっぱり AWS のオートスケールはサーバ運用の大きな課題を解消してくれる強烈なツールだよな。と感じて、これを活用しつつちゃんと動くプラットフォームを確立したいと思い始めた。 オートスケールを使ったシステムを作ろうとする時、最大の課題は複数インスタンス内のファイル同期の担保だろう。複数のインスタンスでディレクトリ内が共有できないと、インスタンス内でファイルを生成する時にインスタンスごとにファイルの有無が生じて不具合が起きる。 基本的にはインスタンスはいつ消滅してしまうかわからないものとして、インスタンス以外の場所に保持されていて、同期が取れるようなNFSとか共有ストレージのようなものが必要になる。そうなるとやっぱり落ちる確率の低い S3 をストレージ代わりに使いたくなる。ということで <a href="http://code.google.com/p/s3fs/" target="_blank">s3fs</a> が期待できる。

<a href="http://code.google.com/p/s3fs/" target="_blank">s3fs</a> は S3 を外付けストレージ化できるのは知っていたが、複数インスタンスでも共有して使えるよと <a href="http://blog.hansode.org/archives/52009455.html" target="_blank">Wakame の中の人が書いていた</a>けど、検証した結果が載っていたわけではなかった。あと、S3 の管理コンソールからファイルを置いた時でもインスタンス上から見えるかどうか気になったので、そこら辺もろもろ確認したかったので検証した。

結論的には、複数インスタンスで共有できるし、S3 にファイルを置いてもインスタンス内から確認できた。結構反映速度も早かったので、実用的に使えるツールだということが分かった。

セットアップからテストの手順を示しておく。 検証した s3fs のバージョンは 1.69。

参考URL

*   http://blog.hansode.org/archives/52009455.html
*   http://rock-and-hack.blogspot.jp/2011/08/s3fs-amazon-linux.html
*   http://lab.eli-sys.jp/2013/02/03/amazon-s3をファイルシステムとしてマウントする/
*   http://www.mornin.org/blog/scalable-wordpress-amazon-web-services/

## インスタンス起動、s3fsをセットアップ

S3 のバケット内にマウントさせたいディレクトリを作っておく。 AWS Management Console にてバケット内にディレクトリを作成する。今回は deesea-bucket 内に test-s3fs ディレクトリを作った。

AWS Management Console からインスタンス1つを起動(以下、Instance A とする)。AMI は AWS 提供のにした。 起動後 ec2-user で ssh login。作業ユーザは ec2-user。s3fs のインストール手順に従って必要なものをインストールする。

<pre class="brush: bash; title: Instance A; notranslate" title="Instance A">$ sudo yum -y install gcc
$ sudo yum -y install libstdc++-devel
$ sudo yum -y install gcc-c++
$ sudo yum -y install fuse
$ sudo yum -y install fuse-devel
$ sudo yum -y install curl-devel
$ sudo yum -y install libxml2-devel
$ sudo yum -y install openssl-devel
$ sudo yum -y install mailcap </pre></p> 

s3fs をダウンロードしてインストール。

<pre class="brush: bash; title: Instance A; notranslate" title="Instance A">$ wget http://s3fs.googlecode.com/files/s3fs-1.69.tar.gz
$ tar xvzf s3fs-1.69.tar.gz
$ cd s3fs-1.69/
$ ./configure --prefix=/usr
$ make
$ sudo make install </pre></p> 

s3fs コマンドが使えるようになっているはず。 <a href="http://code.google.com/p/s3fs/wiki/FuseOverAmazon" target="_blank">s3fs の Wikiにある手順</a>を見つつ設定する。

<!--more-->

<pre class="brush: bash; title: Instance A; notranslate" title="Instance A">$ touch /home/ec2-user/.passwd-s3fs
$ vi ~/.passwd-s3fs
accesskey_id:secret_accesskey_id
:wq
$ chmod 600 ~/.passwd-s3fs </pre>

accesskey\_id と secret\_accesskey_id は自分のに置き換えてください。

passwd-s3fs は /etc/passwd-s3fs に置いても良い。記載内容は同じ。その場合ファイルのアクセス権は、

<pre class="brush: bash; title: Instance A; notranslate" title="Instance A">$ sudo chmod 640 /etc/passwd-s3fs </pre>

とする。

FUSE の設定で allow_other を使えるようにする。 <pre class="brush: bash; title: Instance A; notranslate" title="Instance A">$ sudo vim /etc/fuse.conf
user_allow_other # comment out
:wq
</pre>

## s3fs コマンドでマウントする

Instance A 内にテスト用のディレクトリを作って S3 とマウントする。 <pre class="brush: bash; title: Instance A; notranslate" title="Instance A">$ cd $ mkdir test-s3fs
$ ll
drwxrwxr-x 2 ec2-user ec2-user 4096 5月 29 06:10 2013 test-s3fs
$ s3fs deesea-bucket:/test-s3fs /home/ec2-user/test-s3fs -o allow_other
$ df -h
Filesystem Size Used Avail Use% マウント位置
/dev/xvda1 7.9G 1.2G 6.7G 15% / tmpfs 298M 0 298M 0% /dev/shm s3fs 256T 0 256T 0% /home/ec2-user/test-s3fs </pre>

s3fs がマウントされて 256T あるように見えればOK。

## ファイルが作成できるかテスト

ファイルを作ってS3で見えるかどうかテスト。

<pre class="brush: bash; title: Instance A; notranslate" title="Instance A">$ cd /home/ec2-user/test-s3fs
$ sudo touch temp.txt </pre>

S3 の Management Consle 上の deesea バケット内 test-s3fs ディレクトリ内に temp.txt が見えればOK。

## 複数インスタンス間で共有テスト

Instance AをAMI化。そのAMIからインスタンス1つ起動(以下、Instance Bとする)。 Instance B に ec2-user で ssh login。s3fs コマンドでマウントする。 <pre class="brush: bash; title: Instance B; notranslate" title="Instance B">$ s3fs deesea-bucket:/test-s3fs /home/ec2-user/test-s3fs -o allow_other
$ df -h
Filesystem Size Used Avail Use% マウント位置
/dev/xvda1 7.9G 1.2G 6.7G 15% / tmpfs 298M 0 298M 0% /dev/shm s3fs 256T 0 256T 0% /home/ec2-user/test-s3fs </pre>

Instance A と Instance B で同じS3ディレクトリをマウントしている。 Instance A でファイルを作って Instance B から見えるかどうかテスト。 Instance A でファイルを作る。

<pre class="brush: bash; title: Instance A; notranslate" title="Instance A">$ sudo touch CanYouSeeMe.txt </pre>

Instance B では以下コマンドを実行して、いつファイルが見えたかを測定。

<pre class="brush: bash; title: Instance B; notranslate" title="Instance B">$ while /bin/true; do date; ls -l /home/ec2-user/test-s3fs/; echo -e; sleep 1; done </pre>

結果 : 1秒以内で反映された。

Instance A でファイルを削除する。

<pre class="brush: bash; title: Instance A; notranslate" title="Instance A">$ sudo rm CanYouSeeMe.txt </pre>

結果 : 1秒以内に反映された。

インスタンスBでファイル作成/削除しても同様の結果だった。

## S3上でファイルを作ってインスタンスで確認する

Management Consle から6MBファイルを deesea-bucket test-s3fs ディレクトリにアップロード。 Instance A, Instance B 共に以下のコマンドで確認。

<pre class="brush: bash; title: ; notranslate" title="">$ while /bin/true; do date; ls -l /home/ec2-user/test-s3fs/; echo -e; sleep 1; done </pre>

アップロード後、1秒以内に確認できた。

S3上でファイルを削除しても同様の結果だった。

### 問題

困ったこととして、s3fs コマンドでマウントした時のディレクトリは root:root 所有になってしまうことと、S3からアップロードしたがファイルのパーミッションが 000 になっていること。 <pre class="brush: bash; title: Instance A; notranslate" title="Instance A">$ ll
drwxrwxr-x 2 ec2-user ec2-user 4096 5月 29 06:10 2013 test-s3fs $ s3fs deesea-bucket:/test-s3fs /home/ec2-user/test-s3fs -o allow_other
$ ll
drwxrwxr-x 1 root root 0 1月 1 00:00 1970 test-s3fs </pre>

元々 ec2-user:ec2-user 所有だったディレクトリが s3fs したら root:root 所有になった。

ディレクトリのパーミッションは s3fs コマンドでマウントする前に chmod コマンドで変更すればそのパーミッションは維持してくれる。

<pre class="brush: bash; title: ; notranslate" title="">$ sudo chmod 707 test-s3fs
$ ll
drwx---rwx 2 ec2-user ec2-user 4096 5月 29 06:10 2013 test-s3fs
$ s3fs deesea-bucket:/test-s3fs /home/ec2-user/test-s3fs -o allow_other
$ ll
drwx---rwx 1 root root 0 1月 1 00:00 1970 test-s3fs </pre>

パーミッションが維持されているが、ディレクトリの所有は root:root。

そして、S3 の Management Console からファイルをアップロードすると、

<pre class="brush: bash; title: ; notranslate" title="">$ ll
test-s3fs ---------- 1 root root 1021256 7月 8 22:25 2013 CanYouSeeMe.txt </pre>

パーミッションが 000。 これではサーバプロセスからこのファイルを読むこともできないので、例えば Web サーバ上からこのファイルを読もうとするとパーミッションがなくエラーになる。

良い解消方法はないものか。知っている方いらっしゃれば教えて頂きたいところ。

## 結論

インスタンスとS3はすべて同期が取れる。思ったよりも早い反映速度だった。 S3からファイルをアップロードする方法はパーミッションが付与されないので、実質運用できない。 パーミッション部分が自由に付与できるような方法があればインスタンスにログインせずに、S3からファイルの更新が出来る環境が実現できる。