---
layout: post
title: "新しいサービス OpsDeliver API Server をリリースしました"
date: 2015-12-03 19:30:00 +0900
comments: true
categories:
  - Business
  - Startup
  - OpsDeliver
tags:
  - opsdeliver
  - shakesoul
  - release
---

[会社でニュースリリースを出しました](http://www.shakesoul.net/2015/12/03/release-opsdeliver-api-server.html)が、今日、新しいサービスをリリースしました。

[OpsDeliver API Server][opsdeliver-apiserver] といいます。

![OpsDeliver API Server Web](/images/2015/12/20151203-web-apiserver-opsdeliver.png)

OpsDeliver の派生サービスでソフトウエアパッケージの形態で提供します。


# 導入第一弾は「さくらのクラウド」

導入いただいた第一号は [さくらのクラウド][sacloud]さんです。

[さくらインターネットさんは東証一部へくら替えしたばかり](http://www.sakura.ad.jp/press/2015/1120_section_transfer/) でなにかと今勢いがありますが、
事例としての紹介OKいただきました。ありがとうございます！

前回お仕事の話([開発したさくらのクラウド新サービスがリリースされました][berfore-post])で紹介した [クラウドカタログ][cloud-catalog] の中で利用されています。

# ターゲット

IaaSやホスティングサービスをされているサーバ事業者や社内でプライベートクラウドを運用されているインフラ担当のチームにマッチすると思っています。

特に、DevOps系のサービスをこれから提供したいと思っていらっしゃるIaaS事業者さんに是非使っていただきたいところです。

# どんなサービス？

<!-- more -->

OpsDeliver API Server は、Chef の Cookbook をシンプルに実行・管理するための REST API サーバです。

![Chef logo](/images/2015/06/20150602-chef-logo.png)

実行した Cookbook の設定内容や実行結果をサーバに保存するため、作業の振り返りやチーム共有ができるようになります。よりアプリケーションのセットアップに注力した機能を持っています。

## with IaaS

IaaS と絡めると OpsDeliver API Server から IaaS API 叩いてサーバを起動させ、立ち上がったサーバにアプリケーションのセットアップができます。

ユーザからは、ワンクリックして待つだけで、自分の指定したパラメータ通りにセットアップされたサーバができあがる。という体験を届けることができます。
Linux オペレーションに高い壁を感じてしまうユーザたちには魔法のようなサービスを提供できます。


# なぜ作ったか

Chef の利用実態は Chef Server よりも knife-solo を使っている方が多いと思います。

knife-solo はいつどんなアプリをどんなパラメーターででセットアップしたかは、ログの履歴保存の機能がないので振り返れません。
個人で使う分には良いけどチームで作業履歴をシェアしたい場合には不向きでした。

そこら辺はやはりDBに残しておきたい。作業内容や履歴のチーム共有ができます。

REST API といインターフェイスにすれば、コマンドライン以外のブラウザでのインターフェイス提供も容易になります。
IaaS のWebUIのコントロールパネルにサービスを追加しやすいです。


# DevOps をチームで促進させるためのツール

今 DevOps は時代の大きな流れになっています。よりインフラがソフトウエア化され自動化へのアプローチが進むでしょう。

人が手作業で行っていた仕事はなくなり、代わりにソフトウエアを使って劇的に効率化が進む時代になることは間違いないことです。

今回の DevOps API Server はチームやインフラに詳しくないユーザにも DevOps のメリットを届けるためのツールになります。

# Chef トレーニングも実施中

とは言え、まだ Chef を導入して日常的にバリバリ使えているところは少ないかもしれません。
学びたいけどなかなか時間がなくて、、、という方々のためにパブリックな学ぶ機会を設けています。

Chef テクニカルトレーニング: [http://chef-training.connpass.com/](http://chef-training.connpass.com/)

今年は12/8で最終回ですが、今回のフィードバックから来年のカリキュラムを組む予定です。
興味のある方は Connpass グループのメンバーになっていただければです。

また、[企業研修用のプログラム](http://www.shakesoul.net/tech-lecture) も会社としてやっていますので、社内研修を検討されている方はチェックいただければです。

# 今後もリリース続きます

今回は会社としてのポートフォリオの1つの分野にポジションするサービスになります。
今後、異なるポジションのサービスをリリースしてく予定です。

どんなことを考えてリリースしていくのか、そこら辺の話はまた今度。

今後も要チェックでお願いできればです。

[opsdeliver-apiserver]: https://apiserver.opsdeliver.com
[sacloud]: http://cloud.sakura.ad.jp/
[berfore-post]: /2015/11/11/launch-cloud-catalog/
[cloud-catalog]: http://cloud-news.sakura.ad.jp/cloud-catalog/