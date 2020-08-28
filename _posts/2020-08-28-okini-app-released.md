---
layout: post
title: "Okini(オキニ)という買い忘れ防止アプリをリリースしました - 初 Flutter 開発 -"
date: 2020-08-28 18:00:00 +0900
comments: true
categories:
  - Tech
tags:
  - Okini
  - オキニ
  - 買い忘れ防止アプリ
  - flutter
  - development
  - push notification
  - スマホアプリ
  - iOSアプリ
  - Androidアプリ
description: 自分の何気ない日常の課題 お気に入り商品の会話スレを防止するためのアプリ Okini (オキニ) をリリースしました。初めて Flutter で開発したiOS/Androidアプリになります。
image_out: https://lh3.googleusercontent.com/nEFl8louMq7-saFsgdqhyyIFTZfyJslgLruG0JYdiiqWUAqmjKsdkmSOwFrWLd_HQ5z0UjFEWBLc3uEbu-fr8bLawhhsrPIjCe0lW9hkBbRO0r7Ta8fVevq1H-QC5819m2Q-rHYPfLk
---
[![Okini](https://lh3.googleusercontent.com/nEFl8louMq7-saFsgdqhyyIFTZfyJslgLruG0JYdiiqWUAqmjKsdkmSOwFrWLd_HQ5z0UjFEWBLc3uEbu-fr8bLawhhsrPIjCe0lW9hkBbRO0r7Ta8fVevq1H-QC5819m2Q-rHYPfLk=w380)](http://okini.mystrikingly.com/)

なにげない日常の困りごと、お気に入り商品の買い忘れを防止するアプリ Okini (オキニ) をリリースしました。初めて Flutter で開発したiOS/Androidアプリになります。

[![Okini](https://lh3.googleusercontent.com/nEFl8louMq7-saFsgdqhyyIFTZfyJslgLruG0JYdiiqWUAqmjKsdkmSOwFrWLd_HQ5z0UjFEWBLc3uEbu-fr8bLawhhsrPIjCe0lW9hkBbRO0r7Ta8fVevq1H-QC5819m2Q-rHYPfLk=w180)](http://okini.mystrikingly.com/)

[![App Store](https://lh3.googleusercontent.com/0ntrwrDbEBgmbkEJ6SuDOR1VORrFYBeOPRb2SsJn5gjaC_nJp0wAS_W2ynJUFuxl4zcZIswtji3H9cHC-P2rSh8SkqLTwBlBj9Rre4EoU2VMVFRbxTkJjI5WIVsMqgN5SMk5gzothyg=w160)](https://apps.apple.com/us/app/id1526396439)
[![Google Play](https://lh3.googleusercontent.com/n3zQMRJD-gWFZgOKi0i3CZ25WEkTufhAZpyJKHFQE_OIbIVpXFRkiVlymIieiE9uPXrE7Pd1G2qIwe26sikV_KH_hFiBdU8tBJ1rH-vIk02NMuWdQqeEkY4iGzXWq7aIrxVv3hN2wr4=w160)](https://play.google.com/store/apps/details?id=st.players1.Okini)

<!-- more -->

# どんなアプリ？

家にいつも置いておきたいお気に入りの商品がなくなる前に買い時をお知らせする、買い忘れ防止アプリです。

以下、App Store, Google Play での説明文。

> 困った。。。😢
「いつものビールが夕飯に飲めなかった」
「お風呂に入ったらシャンプーが切れたことに気づいてしまった」
「甘いものが食べたくなったけど、大好きなチョコレートがなくなっていた」
「コンタクトの洗浄液がない！まずい！」

> 93%が買い忘れ体験
いつも家にないと困ってしまうモノ、ありますよね。
なくなる前に買っておきたいのですが、ついつい忘れてしまい、家に常にあるようにするのはなかなか難しいことです。
実に93％の方が買い忘れをして困った経験があります。(当社アンケートによる)
「買い忘れた。。」をなくします
アプリにお気に入りの商品を登録すると、使い切りそうな時期にお知らせして買い時を逃しません。

# 自分の困りごとから生まれたアプリ

近所のスーパーに買い物行ったり、ヨドバシなどのネットでも日用品やら家で飲むワインやらを買うんですが、なくなってしまうと不便ですよね。

例えば、シャンプーなくなったことに風呂入った後に気づくとか、夕飯時にちょっとしかワインが残っておらず晩酌が楽しめなかったりとか、そんな経験をしてなんとかしたいなと思いました。

継続的に家にあってほしいものってプリンターインクとか日用品以外にも結構あって、全部の残り具合を確認して覚えておくのも手間だし、そんなことを覚えておくために自分の記憶領域を使いたくないなぁ。などと思っていました。

## 最初作ったのは Ruby スクリプト

そこで思いついたのが、使い切ってしまう前に「そろそろ買い時ですよ」と通知してくれる仕組みで、大体の使用期間はほぼ一定だろうから、購入日から使用期間を足して使い切る数日前にメールでお知らせするスクリプトを Ruby で書きました。

{% gist 32f7aa761ad6ad6aaeaa216357360018 okini-sample.rb %}

こんなスクリプトで、

{% gist 85e8ba877dcc3d25b8c2445f210ce9b9 item-list.json %}

お気に入り商品管理用の JSON を一行ずつ読むという具合で、買い時だったらメールを飛ばします。

このスクリプトを cron で毎日回しました。

これはかなり効果があって、都度、購入日の JSON を書き直したり、使用期間の調整をする手間はありますが、買い忘れることはなくなりました。

## アプリ化へのアプローチ

### やっぱりアプリで通知させたい

使い続けてくると、JSON ファイルを都度更新するのは手間だし、メールっていうのも古いし、今どきならアプリのプッシュ通知でお知らせは受け取りたいなと思うようになりました。

### すでにアプリないの？

「買い忘れ防止」などのキーワードで App Store を検索してみると、いくつか出ました。
見るとだいたい、買い物メモの機能を持たせるもので、 **買い時を自動的にプッシュ通知でお知らせしてくれる** 仕組みは持っていませんでした。

Webサービスでも検索したのですがこちらはもっと少なく、使用期間を調節できるようなものはありませんでした。

### 実装に入る前にアンケート

個人的には買い忘れの経験とか買い時通知の便利さを実感してたのですが、アプリを開発するほどの価値はあるのかどうか、調べてみようと思いました。

クラウドワークスのアンケート機能使って30名にアンケート取りました。この機能は便利でWebで単価(支払金額)とアンケートを作ると翌朝には30名分集まっていました。

アンケート回答より、買い忘れの経験が93％の人であるとか、それを防ぐためのツールがメモと記憶くらいしかないことがわかって、それならこのアプリを出す意味がありそうだと思い、仕事が一段落して時間ができた時に開発を始めました。

# Flutter 初開発

最初 Swift UI で iOS アプリとして作ろうか思っていたのですが、今ならマルチプラットフォーム対応の環境もありそうだけど、もう Titanium Mobile はないよなと思ったりして、SNSでつぶやいたら返事くれた人はこぞって Flutter を勧めました。

調べてみると Google が出している、マルチプラットフォーム対応で iOS/Android アプリが1つのコード作れちゃうのはもちろん、Firebase との連携も同じ Google だからスムーズらしい。

もともとバックエンドは Firebase を使おうと思っていたので、Flutter を採用。

Dart は初で概念が Xcode/StoryBoard とは色々違うようですが、学びながら開発を進めることにしました。

そして、開発に取り組むこと1.5ヶ月。Okini は初めての Flutter で開発したアプリとしてリリースされました。

## 魔法を一つだけ

元々シンプルな処理なので、つまづきそうな機能はプッシュ通知くらいかと思って開発を進めたのですが、手元で動かしてみると「ワオッ」というような体験を1つさせたい気がして、1つ機能を足しました。

商品のバーコードをカメラで読み取り、商品名や画像を表示するという、テキストを入力することなく登録することのできる、入力補完機能です。

こんな感じでカメラでバーコードを読み込むだけで、自動的に商品情報を入力します。

![video screen capture](https://lh3.googleusercontent.com/_Z2fUH7JP_QFHOhIhnQ3w8hNV4JQYHKOsnK6iIU0z1s-fnuJ6ZIYS0lxrTXxFNf-rqBFj275LrSX3NVdHzL07nyPs-NShNUWa5rcjrKlpQOo58Qv7P-piOBfEoZb-5ydDoCyAra1yV4)

# 最近は「自分ごと」の課題解決にフォーカス

最近は自分の体験した課題を解決するためにサービスアイデアを考えるようにしています。

## リーンスタートアップもデザインシンキングも不確実性は残る

経験上、世の中のトレンドや「xxx業界が熱い」みたいな情報に基づいてサービスを練ったとしてもほぼ間違いなく外れる。何しろユーザーの課題への理解が低い状態で的外れなものを開発してしまう可能性大。(市場のトレンドを抑えるのは資金調達する際にはアピールポイントかもしれないけど、結果的に成長できずに苦しくなる)

課題を明確化するために、見込み顧客へのインタビューやデザインシンキングで言うところの観察をするわけだけど、見込み顧客を正確に見つけられるのか、またインタビューしている or 観察している対象は果たして本当に見込み顧客なのだろうか、不確実な要素は多々含まれる。

## ターゲットは自分

なので、自分が日常の中で経験し「不便だ」とか「もっと便利にできないの？」と思ったことをベースにすれば、自分自身がターゲットだし、解決したいことなので不確実性は他人をターゲットにした場合に比べて少なくできる。


もしかして市場は小さいかもしれないけど、最悪自分が使って便利だと思えれば作った甲斐も感じられる。

心情的には「小さく試す」ために最小限の機能でリリースしたいが、しばらく触っていくうちに足りない部分を実感できる。

自分の課題を満足できるレベルでしっかり解決することを目指すように開発したほうが良い気がしている。特にスマホアプリはUXが重要なので、何がしかの小さな魔法を含めたいと思うところ。

# ということで Okini よろしくです

家に切らしたくないものがある方で常に買い物しなきゃいけないような、一人暮らしや買い物担当の方に使っていただければです。

よかったらインストールしてみてください。


 [![Okini](https://lh3.googleusercontent.com/nEFl8louMq7-saFsgdqhyyIFTZfyJslgLruG0JYdiiqWUAqmjKsdkmSOwFrWLd_HQ5z0UjFEWBLc3uEbu-fr8bLawhhsrPIjCe0lW9hkBbRO0r7Ta8fVevq1H-QC5819m2Q-rHYPfLk=w180)](http://okini.mystrikingly.com/)

 [![App Store](https://lh3.googleusercontent.com/0ntrwrDbEBgmbkEJ6SuDOR1VORrFYBeOPRb2SsJn5gjaC_nJp0wAS_W2ynJUFuxl4zcZIswtji3H9cHC-P2rSh8SkqLTwBlBj9Rre4EoU2VMVFRbxTkJjI5WIVsMqgN5SMk5gzothyg=w160)](https://apps.apple.com/us/app/id1526396439)
 [![Google Play](https://lh3.googleusercontent.com/n3zQMRJD-gWFZgOKi0i3CZ25WEkTufhAZpyJKHFQE_OIbIVpXFRkiVlymIieiE9uPXrE7Pd1G2qIwe26sikV_KH_hFiBdU8tBJ1rH-vIk02NMuWdQqeEkY4iGzXWq7aIrxVv3hN2wr4=w160)](https://play.google.com/store/apps/details?id=st.players1.Okini)
