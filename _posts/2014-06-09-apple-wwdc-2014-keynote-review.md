---
layout: post
title: "Apple WWDC 2014 キーノートを見た雑感"
date: 2014-06-09 20:00:00 +0900
comments: true
categories: Apple
---

![WWDC 2014](https://devimages.apple.com.edgekey.net/wwdc/images/wwdc14-home-branding.png)

[Apple WWDC 2014 キーノート][keynote]を見ての雑感をまとめておく。
[keynote]: http://www.apple.com/apple-events/june-2014/

柱としては、新OS X、iOS 8、Swift といったところだろう。

<!-- more -->

# OS X Yosemite

   * iOS 7 のフラットデザインを OS Xにも踏襲
   * ダークモードで盛り上がるｗｗｗ
   * Notification Center のエリアが大きくなって情報量が多く見やすくなった
      * 今まで使われなかったWidgetをここに配置している感じ
   * Spotlight が launch tool : Alfred みたいにファイル検索以外の機能も持つ
      * キーワードが色々なアプリを横断的に探してくるっていうスタンスは良いなぁ、ユーザビリティ向上のお手本みたいだ
   * iCloudがGoogle Driveみたいになった、Dropbox, Google Driveに続くクラウドストレージサービスとして使いそう
   * Mail : PDFファイルに自分のサイン加えて送れるとか、これは便利だなぁ
      * 手書きがアイコンに変換されるのもトラックパッド使いには良さそう
      * ってことは Keynote, Pagesとかにも使えるよな
   * Safari : アイコンが iOS と同じになった
      * タブのエリアが横スクロールするのは良いな、ここらへんも画面が小さいiPhoneからの踏襲
      * サムネイル画像がタブとブックマークに用いられているけど、サイトが更新されたらどれくらいで反映されるんだろう
   * Hand off : iPhone と Mac をシームレスにつないで編集できるとかはアップルならではだな
   * SMS/Phone call がMacで取れるとか、これは良い！作業中のMacで一元的に住むのは良い
   * SafariのMac/iPhoneの見ているページのシームレスな共有は素晴らしい


iPhoneとMacの両方を持つユーザに対する不便さをなくし、よりシームレスに連携できるようになった。
両方を持っていないとこのスムーズな体験はできない。
ある程度スマホ販売の成長率が鈍くなってきた中で、これ以上の発展の仕方としてはiPhoneもMacも持ってもらう戦略になる。

元々はPC側を母艦にしていたけど iPhone ならではの機能や使い方がはっきりしてきて、パワーバランスをPCと均等にしようということなのだろう。ジョブズがいた頃のプレゼンで "Back to Mac” を掲げたが、それをさらに進めた印象を持った。

# iOS 8

   * Notification Center : ロックスクリーンからアクションできるのは、従来のステップの多さを削るシンプルさを求めるアップルのスタンスがよくでている
   * メールの書き途中を下に置いておけるのは良いなぁ
   * Spotlight : Googleから検索窓を奪うか
   * Quick Type : 日本語ではおなじみだけど、英語では直打ちだからな、そういう意味では英語のサジェスチョン入力機能ないのかなと思っていたら出たｗ
   * Messages : 音声が来るとロックスクリーンでも耳に当てると再生して、もう一度当てると録音になる。知らないとわからないけどここまで手順をそぎ落としてしまうのはアップルらしくて良い。その機能がほんとうに良いのかは別にしてシンプルになろうとするアプローチを徹底しているスタンスが好きだ。
   * Health はアップルがやる必要があるのかなぁ、Kitはヘルス系アプリを作りやすくしてくれるけど、流行りに流され気味な気もする
   * Family Share は買ったアプリや曲もシェアできるとなると売上は減るけど、家族でmp3をコピーしあっていることを考えれば良いのかも、家族ごと囲えれば少しの損失も良いよと
   * Macには iPhotoやめて iOS の Photos を移植って感じか
   * iCloud安くなったなぁ 20GB $0.99/monthってホントかいな、20G 100円/月ってこと？1200円/年、これくらいなら使ってもいいなぁ、Google Driveに対抗してだろうけど、単価は高いが容量をより下の20GBにして価格$1.99でスペックも価格も下まわるプランを出した

## Dev

   * 1200万Appか、そんな数になったんだなぁ

## App Store

   * 確かにアプリのビデオは良い、スクリーンショットは加工が激しくもはやスクリーンショットになっていなかったからな
   * TestFlight もテストをどうするかに対してアップル自身が解決策を示した

## Extensions

   * これも個人的に要望していた点。facebook/twitterだけではもったいないし、そこにアプリを入れたい
   * アプリ間がよりシームレスに連動することができるようになった
   * タッチIDのサードパーティ解放、これも開発には嬉しい、ただiPhone5Cの人たちもいて TuchID使える割合が少ないから何かのもう一つの手段として使う感じになるか

## HomeKit

   * 最も身近にあるデバイスだからこそ成り立つのか、iPhoneを使ってスイッチまで行かずに制御する、病院とか老人ホームも使えそう

## CloudKit

   * これはMBaaSだな、アップルも参入
   * Freeでリミットありだから儲ける気はなさそうだけど、反応を見て価格やスペックを変えてくるだろう

## Metal

   * Unityとかのゲームエンジン向けの特別に薄いレイヤのAPIを提供するってことなのかな？

# Swift

残り10分少々での Swift 登場

   * Swift の紹介の仕方を見るとObjective-Cと同列だけど、実際は Objective−C の上で走るよな
   * コーディングしながらプレビューできるのは良いなぁ

# 全体的な印象
  * 今回の発表のメインは 新 OS X  と iOS8 の2台柱
  * めいいっぱいですべてのプロジェクトを紹介した感じ
  * すべてソフトウエアの話し、プレゼンテーターはほぼ  Craig Federighi 1人
  * 時間が2時間近くになったのは久々では
  * OS X, iOS8 連続的改善の賜物、実はアップルの強みはゼロから産みだすというより、ユーザにとって不便な部分をちゃんと分かって改善してくること、より完成度が上がっていく
  * Swiftは唯一の新しいネタ

# プレゼンテーションの観点から

個人的にはアップルのキーノートはスティーブ・ジョブズが登壇していたころから自分のプレゼンの参考にいつもさせてもらっているので、「良質なプレゼンテテーションを学ぶ」という観点でも見ている。

<a href="http://www.amazon.co.jp/gp/product/B00EH93MO6/ref=as_li_ss_tl?ie=UTF8&camp=247&creative=7399&creativeASIN=B00EH93MO6&linkCode=as2&tag=dsea-22">スティーブ・ジョブズ 驚異のプレゼン</a><img src="http://ir-jp.amazon-adsystem.com/e/ir?t=dsea-22&l=as2&o=9&a=B00EH93MO6" width="1" height="1" border="0" alt="" style="border:none !important; margin:0px !important;" />
この本を読んで、ジョブズのキーノートを何回も何本も見てプレゼンテーションの練習をした。

今回も相変わらず分かりやすいスライド、よどみのないプレゼンテーションの流れではあるが、今まで90分を守っていたものが2時間近くまで延長。内容も詰め込み過ぎな感が否めない。

ちなみに今まで90分間にしていた理由は観客の集中力が持たないためで、それでも途中でもやビデオを挟んだりして退屈にならないようにしながらも何とか90分まで伸ばしていた。それが今回117分、ほぼ2時間。

観客の集中力を犠牲にしてまで盛り込むべき内容だったのだろうか。観客は終わった後にどんなトピックが印象に残っただろうか。

おそらくインパクトとしては Swift だが、紹介に使った時間は10分以下。インパクトと時間配分が一致しない。おそらくそこまで意図的に考えられなかったのだろう。

新 OS X, iOS 8 は確かに機能数としては多いだろうが、世にアピールしたいポイントは少なくしないと訴求力がなくなる。
Swiftは唯一まったく新しいものとして登場できたと思うが、残り10分少々の段階での登場だったのでもっと時間を割いて、OS X, iOS 8 に続く3つ目のメインとしてスポットライトを当てるべきだったと思う。

ジョブズの頃にはストイックなまでに突き詰めていたプレゼンテーションの質が落ちてきている気がする。

# キーノートから見るアップルの焦り

2時間近くの動画を見終わって最初に思った印象は、アップルは選択と集中ができなくなってきているのかも知れないな。と思った。

**シンプルさの追求** はアップルが今まで継続してきた文化であり強みでもあるのだけど、選択ができずに同列にいろいろな機能を出してしまっているとしたら、ちょっとした危機なのかもしれない。

様々なプレッシャーに対して、焦っているとたくさん並べて充実ぶりをアピールしてしまうのは理解しやすい。ただそれは本当に届けたいものか何か、ユーザの心に残ってほしいものは何かがなくなってしまう。マーケティングの観点からもまずいが、それを意識できなくなってしまっている状況自体がまずい。

新機能の方向性としては正しいと思うが、今後その焦りが自らを苦しめることにならなければ良いなと思った。