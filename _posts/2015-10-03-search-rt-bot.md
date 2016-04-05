---
layout: post
title: "キーワードにマッチする Tweet を Retweet する bot 作った"
date: 2015-10-03 15:00:00 +0900
comments: true
categories:
  - Tech
tags:
  - bot
  - twitter
  - ruby
---
<h1><i class="fa fa-twitter-square fa-5x"></i></h1>

前回の [自動Retweet bot 作った](/2015/09/11/twitter-rt-bot/) のに続き、今度はあるキーワードにヒットした Tweet を 自動 Retweet する bot を作った。

自分のブログ [hirofukami.com](http://hirofukami.com) について Tweet されたものを自分のフォロワーにもアピールしたいかなと思ったのがきっかけ。
評価はポジティブでもネガティブでも問わないという、オープンスタンスで。

tweet id 情報を手元に置きたくないのでまずは Retweet して、すでに Retweet していた場合はエラーで逃げてしまうように。

今回も Ruby の [gem twitter](https://github.com/sferik/twitter) を使って簡単 Ruby スクリプト。
手元の MacBook の cron で動かしています。

環境

* MacBook 12inch Retina OS X El Capitan
* Ruby 2.0.0 (rvm)

以下、gist ソース

<!-- more -->

{% gist b03f4fef85a06267faca search-retweet-bot.rb %}


# 解説

Twitter REST APIs から [search/tweets](https://dev.twitter.com/rest/reference/get/search/tweets) を使って、最近のキーワードにヒットしたtweetを取得する。

tweet が自分自身以外だった場合(screen_name で引っ掛けた)にその tweet を Retweet する。
すでに Retweet していた場合はエラー扱いになるので、 `begin` ~ `rescue` で逃げれるようにしてメッセージだけ出力して終了。


# 使い方

## 事前準備

1. `$ gem install twitter`
1. ~/.bash_profile に Twitter consumer key, access token などを追記

``` bash
## Twitter API
export CONSUMER_KEY="SET YOUR KEY"
export CONSUMER_SECRET="SET YOUR SECRET"
export ACCESS_TOKEN="SET YOUR TOKEN"
export ACCESS_TOKEN_SECRET="SET YOUR SECRET"
```

自分のものに置き換えてください。

これらの情報は [Twitter Application Management](https://apps.twitter.com/) でアプリ追加をした時に発行されます。

## cron に仕込む

`$ crontab -e`

`10 * * * * cd $HOME/Development/Twitter-bot && $HOME/.rvm/wrappers/ruby-2.0.0-p353/ruby search-retweet-bot.rb`

ruby は rvm を使っているためパスの指定が必要でした。毎時10分の時に実行されます。

MacBook を閉じてしまっている時は実行されないので、実質実行時間は自分がPCに向かっている間だけということになります。

# 結果

ということで、以下の様な tweet に対して自動 Retweet していますと。

<blockquote class="twitter-tweet" lang="en"><p lang="ja" dir="ltr">LEAN – インタビューやってみて失敗したあれこれ | Hiro Fukami&#39;s Blog: <a href="http://t.co/WMG3JvjEaP">http://t.co/WMG3JvjEaP</a></p>&mdash; Dyki Ogawa (@Dyki01) <a href="https://twitter.com/Dyki01/status/620883998680571904">July 14, 2015</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

自分もやりたい！けど、敷居が高い。と思った方はついでに動かすことはできるので相談ください。