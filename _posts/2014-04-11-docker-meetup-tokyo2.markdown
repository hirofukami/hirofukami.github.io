---
layout: post
title: "Docker Meetup Tokyo #2 に参加した"
date: 2014-04-11 22:00:00 +0900
comments: true
categories: Tech
---

![Docker Meetup Tokyo #2](https://connpass-tokyo.s3.amazonaws.com/thumbs/7f/d1/7fd16d32367bb52155ca416e032071fc.png)

* イベント概要 : http://connpass.com/event/5640/
* togetter : http://togetter.com/li/653977

最近 Docker Docker と tweet されていて、かなりな盛り上がりを見せている Docker ですが、その Meetup があるらしいと tweet で流れていたので申込んだら80番目くらいで申し込めたので行ってきました。

最終的には100名定員に400名以上が申し込んでとんでもないブームになっていました。
多分、自分と同じように触れてないけど Docker ってどんなふうに使っているの？みたいに把握しておきたい人たちが殺到したのでは？と思うところです。

以下、メモと思ったことのまとめです。

<!-- more -->

## 前半発表内容

### @mainyaa：今からでも間に合うDocker基礎+Docker 0.9概要+Docker 0.10概要

<iframe src="http://www.slideshare.net/slideshow/embed_code/33405582" width="597" height="486" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC; border-width:1px 1px 0; margin-bottom:5px; max-width: 100%;" allowfullscreen> </iframe>
<div style="margin-bottom:5px"> <strong> <a href="https://www.slideshare.net/mainya/dockerdocker09-010" title="Docker基礎+docker0.9, 0.10概要" target="_blank">Docker基礎+docker0.9, 0.10概要</a> </strong> from <strong><a href="http://www.slideshare.net/mainya" target="_blank">Kazuyuki Mori</a></strong> </div>

http://www.slideshare.net/mainya/dockerdocker09-010

Docker の概要を基本からおさらいしてくれる良いスライドでした。

今後本格的に触るかどうか判断しておきたい自分にとっては、概要を理解しつつ最新状況も把握できたとてもありがたい情報ばかりでした。

* デモ
  * Dockerfile に書いてビルドして、追記して
  * ビルド、前回実行部分はキャッシュ、新しい部分だけ新たに実行
  * 差分だけ実行
* Docker前提のCoreOSとか分散処理とかのOSが出てきている
* Docker 0.9
  * LXC 必須ではなくなった、抽象化された、色々なOSでも動ける環境、開発中

### @ten_forward：Linuxカーネルのコンテナ関連機能入門

<script async class="speakerdeck-embed" data-id="7e257d80a38f013120051af8c79ec55f" data-ratio="1.41436464088398" src="//speakerdeck.com/assets/embed.js"></script>

https://speakerdeck.com/tenforward/linux-kernel-falsekontenaji-neng-2014-04-11

* Docker そのものではなく、実現するベースとなっている LXC 中心の内容
* Dockerに関しては最初の発表以上のものはなし
* コンテナの歴史について、LXCの仕組み的な部分もあり

### @naoya_ito：Dockerアプリケーションのポータビリティ

<script async class="speakerdeck-embed" data-id="37db7d10a3900131166e024e11a95d47" data-ratio="1.33333333333333" src="//speakerdeck.com/assets/embed.js"></script>

https://speakerdeck.com/naoya/dockerapurikesiyonfalsepotabiriteiwokao-eru-number-dockerjp

  * [Kaizen Platform Inc.](http://kaizenplatform.in/) の技術顧問の肩書になっていた
    * \# 最近資金調達したはず => やっぱりそうだった http://kaizenplatform.in/pressrelease/2014/03/31/series-a.html
  * Build Once, Run Anywhere どこでもDockerが動けば動かせるよ
  * atlassian/jira プロジェクト管理ツール
    * $ docker run atlassian/jira で社内で動いちゃう
  * Heroku push のタイイングで LXC で作りなおしている、新しいのを作り古いの捨てる

  * ステートレスかつ Shared Nothing
  * 実行/外部環境を明確化・抽象化する
  * 使うアプリを管理するツールを使う
    * その設計方式 The Twelve-Factor App
    * Dockerでどうやるか

* 作ってみた
  * git pull request に紐づくURLを発行して確認したい
  * デモ
    * sinatra で作ったWebアプリ
    * 手順
      * ローカルにレポジトリを作る、git フックが入る
      * 古いコンテナを落とす、新しい docker コマンドを立ち上げる
      * git push して master に上げる
      * 他の人から見て git pull してURLがふられる

## 聞き終わって

### 用途は開発過程でのお手軽サーバ環境

まだプロダクションでは使わないようにと言われているけど、社内では結構ヘビーに使われている。

用途としては社内での開発、検証時に使いたいサーバ環境が多い印象。

差分管理という仕組みも良くて失敗したらロールバックできるし、**試す** という視点ではこの上ないお手軽環境。

### 今までの仮想環境で最も手軽かつ高い汎用性

カーネルが対応したサーバOSならば Docker 動くし、Dockerfile で手軽に環境を移動して別のサーバ上で動かすことができる。今までの仮想環境に比べてこれ以上ない手軽さと汎用性がある。

どんどん作って育てて、捨ててができる。Vagrant に代わって社内の開発環境とか検証環境に使われていくんだろうな。

今までオンプレミスの上でしか動かせなかった Vmware, Xen, VirtualBox(Vagrant) と
仮想サーバOS上の上にさらに仮想環境が作れる Docker。

この違いはとても大きい。ひとつの仮想環境のパラダイムシフトになるくらい大きなインパクトになる。

### Docker を使ったサービスできるんじゃん？

Dockerfile さえあればそれを IaaS 上のサーバに置いて動かせるから、それを置いてあげる Docker(file) Hosting のようなことができる。PaaS ではなくて IaaS on IaaS みたな形態。手元で試したまんまがサーバ上で動かすことができるメリットがある。

ゆくゆく Docker 1.0 が出ればそれをそのままプロダクションとして動かしても良いから、開発環境(ローカル Dockerfile) = ステージング(Docker Hosting) = 本番(Docker Hosting) となってシームレスなデプロイが実現できる。

と思ったら既にいくつかのサービスが既にあった。

http://www.centurylinklabs.com/top-10-startups-built-on-docker/

サービスのプライシングを見るにどこも同じ感じ。通常のホスティングサーバみたいにCPU、メモリ、ディスク容量などのリソースによっていくつかの月額プランがある。
金額は $10/month 〜 $200/month の範囲。

技術的には新しいけど、プライシングのパラメーターが古いと結局現状のホスティング事業者の競争と同じように差別化しにくいから、サービス提供者側は価格を下げる叩き合いになっちゃう気がする。
もう少しユニークな視点で Docker の特徴とかをパラメータに入れられると良いかもね。

### Docker 1.0 が出た時のインパクトを考える

既にこの盛り上がりようなので 1.0 が出て プロダクションとして使ってOKとなれば違った面が一気に活性化されそう。

でも、Docker Hosting は存在するし、サービス内で利用する用途としてはやっぱり現状の共有レンタルサーバの代わりとなるのが思いつきやすい。

現状の共有レンタルサーバの Apache1.0 suEXEC の環境はロリポ事件のようなことが起きてもそんなに改善はなくて、技術的な追求はないからだらだら過ごしてしまっている。

IaaS 1インスタンス 上にたくさんの Docker プロセスを立ち上げて、suEXEC に比べて独立性の高いセキュアな環境を提供できますよ。ということだろう。

あまり革新的なイメージはないけど、国内外のホスティングやさんたちはおそらくやる気がするから単なるWebホスティングサービスだけをすごくやろうとは思わない。

やるとしたら WordPress, Redmine とかアプリを入れて PaaS かな。でも現状共有型でも提供できてしまっているし、内部的にセキュアになるくらいか。

もう少し考えたいところ。

### 今後も楽しみな Docker Meetup Tokyo

今回で400名申込だったので、次回の #3 はどのくらいになってしまうんだろう。

ユーザも増えるだろうし、Docker 自身もすごいスピードでリリースされているので充実した内容になることが楽しみです。

スピーカーの皆様、企画いただいたオーガナイザーの方々へ感謝いたします。ありがとうございました！

