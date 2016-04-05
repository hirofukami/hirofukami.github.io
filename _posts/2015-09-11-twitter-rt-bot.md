---
layout: post
title: "Retweet されたら自動ポストする Twitter bot 作った"
date: 2015-09-11 16:00:00 +0900
comments: true
categories:
  - Tech
---
<h1><i class="fa fa-twitter-square fa-5x"></i></h1>

ちょっと前から [TechCrunch Japan の Twitter](https://twitter.com/jptechcrunch) でキリ番RT bot 運用している様子を目にして、同じようなものを作りたくなった。

Retweet(RT) は Retweet してくれた人の followers には届くが、自分の followers には届かない。
だから、あえて followers にお知らせすることはちょっとした tweet のアピールになるかもと思う。

で、ついでに Favorites(Fav) も付けたいところ。Favorites の方はもっとクローズドなフラグで、tweet した人しか見えないので同じように見せれるといいなと。

土曜の半日で出来た。以下のような仕様

* 大げさにせずとりあえず最新の Retweet されたものを検出
* DBを置くのは大げさなので、テキストファイルにID、RT数、Fav数を書き込む
* Ruby の gem twitter の力を借りる
* 自分の場合、RT数は少なくキリ番まで行かないので、1RTでもついた時点で自動 tweet

Ruby の [gem twitter](https://github.com/sferik/twitter) があるのでこれを使った。

手元の MacBook の cron で動かしています。

環境

* MacBook 12inch Retina OS X Yosemite
* Ruby 2.0.0 (rvm)

以下、ソース

<!-- more -->

``` ruby
require 'twitter'

## login ##
client = Twitter::REST::Client.new do |config|
  config.consumer_key        = ENV['CONSUMER_KEY']
  config.consumer_secret     = ENV['CONSUMER_SECRET']
  config.access_token        = ENV['ACCESS_TOKEN']
  config.access_token_secret = ENV['ACCESS_TOKEN_SECRET']
end

# last since_id from a file
file_id  = open("current.txt").readlines[0].to_i
file_rt  = open("current.txt").readlines[1].to_i
file_fav = open("current.txt").readlines[2].to_i

# access to Twitter API
results = client.retweets_of_me(count: 1)
current_id = results[0].id
current_rt = results[0].retweet_count
current_fav = results[0].favorite_count

unless file_id == current_id && file_rt == current_rt && file_fav == current_fav
  # update a file
  f = File.open("current.txt", "w")
  f.puts current_id
  f.puts current_rt
  f.puts current_fav
  f.close

  results.each do |tweet|
    rt = tweet.retweet_count
    fav = tweet.favorite_count
    text = tweet.text[0..40]
    if tweet.text.length > 41
      text += "..."
    end
    status_url = "https://twitter.com/" + tweet.user.screen_name + "/status/" + tweet.id.to_s

    ## Fav only ##
    if rt == 0 && fav > 0
      status = fav.to_s + "Fav " + "My tweet's response: " + text + " " + status_url
      client.update(status, in_reply_to_status_id: tweet.id)
      puts "Post Response:" + status
    ## RT only ##
    elsif rt > 0 && fav == 0
      status =  rt.to_s + "RT " + "My tweet's response: " + text + " " + status_url
      client.update(status, in_reply_to_status_id: tweet.id)
      puts "Post Response:" + status
    ## both ##
    elsif rt > 0 && fav > 0
      status = rt.to_s + "RT, " + fav.to_s + "Fav " + "My tweet's response: " + text + " " + status_url
      client.update(status, in_reply_to_status_id: tweet.id)
      puts "Post Response:" + status
    end
  end
end
```


# 解説

Twitter REST APIs から [retweets_of_me](https://dev.twitter.com/rest/reference/get/statuses/retweets_of_me) を使って、直近の1件を取得する。

current.txt には TwitterID, RT数、Fav数を各行に書き込むようにする。

TwitterID, RT数, Fav数をそれぞれ比較して、差分があった場合に Tweet する。

Tweet には元の TwitterID の `in_reply_to_status_id` を付ける。こうすることでこの Tweet から元の Tweet が追える。


# 使い方

## 事前準備

1. `$ gem install twitter`
1. ソースと同じディレクトリに current.txt を置く : $ touch current.txt
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

`0 * * * * cd $HOME/Development/Twitter-bot && $HOME/.rvm/wrappers/ruby-2.0.0-p353/ruby rt-fav-bot.rb`

ruby は rvm を使っているためパスの指定が必要でした。毎時0分の時に実行されます。

MacBook を閉じてしまっている時は実行されないので、実質実行時間は自分がPCに向かっている間だけということになります。

# 結果

ということで、以下の様な tweet が自動的にされましたと。

<blockquote class="twitter-tweet" lang="en"><p lang="ja" dir="ltr">19RT, 8Fav - Response to My Tweet: お金のない時の精神的ダメージって想像以上だった。だから、経済的貧困と精神的貧困は同... <a href="https://t.co/j6I9EEF8PJ">https://t.co/j6I9EEF8PJ</a></p>&mdash; Hiro Fukami (@d_sea) <a href="https://twitter.com/d_sea/status/616182566350028800">July 1, 2015</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

思ったよりも簡単に作れました。

Twitter の仕組みは単純で、 API も欲しいものが全部揃っているわけではないですが、わかりやすく楽しく作れました。

自分もやりたい！けど、敷居が高い。と思った方はついでに動かすことはできるので相談ください。