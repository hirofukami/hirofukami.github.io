---
title: '入門Chef 補完 &#8211; chef〜vagrant saharaインストール'
date: Fri, 11 Oct 2013 16:26:20 +0000
author: Hiro Fukami
layout: post
permalink: /2013/10/11/install-chef-solve-way/
categories:
  - Tech
tags:
  - chef
  - develop
  - DevOps
  - install
  - manual
  - opscode
  - ruby
  - sahara
  - vagrant
---
<img class="alignnone" alt="Chef Logo" src="http://docs.opscode.com/chef/_static/opscode_chef_html_logo.png?resize=200%2C154" data-recalc-dims="1" />

来ました Chef です。DevOps は考え方として把握していたけど、具体的にツールとして Chef が登場したし、<a href="http://www.shakesoul.net/smartwordpress" target="_blank">スマートWordPress</a> でも使えそうな気配がしたので、そろそろ把握するにはいタイミングだろうと思って学んでみている。

伊藤直也さん著の、今や Amazon でUnixオペレーティングシステムカテゴリベストセラーNo.1の入門Chef Solo を購入して読了。  
[<img alt="" src="http://ws-fe.amazon-adsystem.com/widgets/q?_encoding=UTF8&ASIN=B00BSPH158&Format=_SL160_&ID=AsinImage&MarketPlace=JP&ServiceVersion=20070822&WS=1&tag=dsea-22" border="0" />][1]<img style="border: none !important; margin: 0px !important;" alt="" src="http://ir-jp.amazon-adsystem.com/e/ir?t=dsea-22&l=as2&o=9&a=B00BSPH158" width="1" height="1" border="0" />  
[入門Chef Solo &#8211; Infrastructure as Code][2]<img style="border: none !important; margin: 0px !important;" alt="" src="http://ir-jp.amazon-adsystem.com/e/ir?t=dsea-22&l=as2&o=9&a=B00BSPH158" width="1" height="1" border="0" />

その通りに自分の環境づくりから始めたものの結構のっけからうまくいかないことが起こった。  
先端的なものは変化が激しいしそんなこともあると思いつつ、色々調べつつ自分なりに起こった事象に対して解消していった。

そんな解消した方法を 入門Chef 補完として紹介します。

最初はChef インストールからローカルで 仮想環境操作を CLI 操作する vagrant のインストール、さらに vagrant のプラグイン sahara インストールまでのインストール手順になります。

今後、また検証が進んで補完内容がたまったら紹介します。

環境は

*   OS X 10.7.5
*   Ruby 1.8.7
*   gem 1.3.7

と、ちょっと古めの MacBook Air です。

<!--more-->

## Chef のインストール手順は Opscode のマニュアルに従う

Chef のインストールは入門Chefでは gem install だが、古い gem だと入らない。gem をアップデートすればよいけど、そもそも ruby 1.9.2 より古いと動作しないかもということもあり、依存しているもののバージョンアップをいちいちするのは手間だし、追い切れない。

Chef のインストール方法は Opscode 提供のシェルを使うと Chef 含め必要なものを丸っとインストールしてくれる。動作確認されているであろう新しいバージョンが入る。  
ここらへんの手順は入門Chefに頼らず一度 opscode.com のマニュアルを読んだ方が良い。

<a href="http://docs.opscode.com/chef/install_workstation.html" target="_blank">http://docs.opscode.com/chef/install_workstation.html</a>

手順は、以下を実行するだけ。

<pre class="brush: bash; title: ; notranslate" title="">$ curl -L https://www.opscode.com/chef/install.sh | sudo bash
</pre>

注意点は現状の環境と別で Mac だと /opt/chef というディレクトリが作られるので、環境に上書きされず Chef 用の独立した環境が1つ追加される。これを個人として許容できれば良いだろう。

作られる階層構造は以下、

<pre class="brush: bash; title: ; notranslate" title="">/opt
   /chef
      /bin
      /embedded
         /bin
         /include
         /lib
         /share
         /ssl</pre>

インストールが終わると、2つの異なる ruby バージョンが存在することになる。

<pre class="brush: bash; title: ; notranslate" title=""># 今までの環境
$ ruby -v
ruby 1.8.7 (2012-10-12 patchlevel 371) [i686-darwin11]

$ which ruby
/opt/local/bin/ruby

$ gem -v
1.3.7

# インストールされた Chef 用の環境
$ /opt/chef/embedded/bin/ruby -v
ruby 1.9.3p448 (2013-06-27 revision 41675) [x86_64-darwin11.2.0]

$ /opt/chef/embedded/bin/gem -v
1.8.24
</pre>

自分は現状の環境のバージョンがちょっと古いしその上で動かしていたものもあるので、独立して設けたほうが支障がなく簡単に導入できるのでこのやり方を使った。

## VirualBox, Vagrant, Sahara の失敗しないインストール方法

本では、Vagran, Saharaまでのインストール手順は、

1.  VirtualBox インストール
2.  gem install vagrant
3.  OSメージダウンロード
4.  vagrant gem install sahara

だけど、この方法は自分の環境では動かなかったり、途中で不整合がでてインストールし直しが起きたりした。  
結果的に手順は以下のようになった。

1.  Vagrant で使いたいOSイメージを決める
2.  OSイメージのVirtualBox Guest Additions バージョンを確認
3.  OSイメージの VirtualBox Guest Additions バージョンと同じ VirtualBox のバージョンをインストール
4.  Vagrant のWebから最新バージョンのインストーラーをダウンロードしてインストール
5.  vagrant plugin install sahara

### VirtualBox のバージョンはゲストOSに依存している

ゲストOSの VirtualBox Guest Additions と VirualBox のバージョンを一致させないと動かない。  
vagrant up するとこんなエラーが出る

> [default] The guest additions on this VM do not match the install version of  
> VirtualBox! This may cause things such as forwarded ports, shared  
> folders, and more to not work properly. If any of those things fail on  
> this machine, please update the guest additions and repackage the  
> box.
> 
> Guest Additions Version: 4.2.16  
> VirtualBox Version: 4.2.18

なので、最初にOSイメージを決めてそれに対応する VirtualBox のバージョンをインストールする必要がある。

<a href="http://www.vagrantbox.es/" target="_blank">http://www.vagrantbox.es/</a> からだと、VirtualBox Guest Additions が示されていないイメージもあったり情報が不足しているが、自分はとりあえず情報がある  
CentOS 6.4 x86_64 Minimal (VirtualBox Guest Additions 4.2.16, Chef 11.6.0, Puppet 3.2.3)  
をダウンロードした。

VirtualBox Guest Additions 4.2.16 なので、VirualBox も 4.2.16 をインストールする。  
<a href="https://www.virtualbox.org/wiki/Downloads" target="_blank">https://www.virtualbox.org/wiki/Downloads</a> からだと最新版 4.2.18 しかないので、  
<a href="https://www.virtualbox.org/wiki/Download_Old_Builds_4_2" target="_blank">https://www.virtualbox.org/wiki/Download_Old_Builds_4_2</a> から 4.2.16 を見つけてダウンロード、インストーラーでインストールする。

### Vagrant インストールは最新版インストーラーを使う

入門Chefでは Vagrant のインストール方法は gem install でとあるが、gem を使うと古い Vagrant がインストールされて、Sahara などのプラグインが追加できなかった。  
Opscode インストール方法で作られた Chef用 gem でも古い Vagrant だった。

<pre class="brush: bash; title: ; notranslate" title="">$ /opt/chef/embedded/bin/gem search vagrant
 vagrant (1.0.7)</pre>

このバージョンではプラグインを追加するコマンドがない。

解決方法は <a href="http://downloads.vagrantup.com/" target="_blank">http://downloads.vagrantup.com/</a> より最新版をインストールする。OS X、Windows、Linuxなどプラットフォームごとにインストーラーが用意されている。  
インストールが終わると sahara プラグインがインストールできる。

コマンドは vagrant gem install sahara ではなく(誤字？) vagrant plugin install sahara  
参考になったURL : <a href="http://qiita.com/hnakamur/items/2c1ae50a23ddc9f7cbe8" target="_blank">http://qiita.com/hnakamur/items/2c1ae50a23ddc9f7cbe8</a>

<pre class="brush: bash; title: ; notranslate" title="">$ vagrant -v
 Vagrant 1.3.4

# sahara プラグインインストール
 $ vagrant plugin install sahara

$ vagrant -h
 Usage: vagrant [-v] [-h] command []

-v, --version Print the version and exit.
 -h, --help Print this help.

Available subcommands:
 box
 destroy
 halt
 help
 init
 package
 plugin
 provision
 reload
 resume
 sandbox
 ssh
 ssh-config
 status
 suspend
 up</pre>

vagrant sandbox コマンドが追加される。これで vagrant sandbox on とかが使えるようになる。

このやり方なら Chef インストールから vagrant が使えるところまで来れるはずです。

本を読んだけど、あれ？うまくいかないなぁとつまずいてしまった方の参考になれば幸いです。

 [1]: http://www.amazon.co.jp/gp/product/B00BSPH158/ref=as_li_ss_il?ie=UTF8&camp=247&creative=7399&creativeASIN=B00BSPH158&linkCode=as2&tag=dsea-22
 [2]: http://www.amazon.co.jp/gp/product/B00BSPH158/ref=as_li_ss_tl?ie=UTF8&camp=247&creative=7399&creativeASIN=B00BSPH158&linkCode=as2&tag=dsea-22