---
layout: post
title: "iOS アプリ開発 SkeletonView ロード待ちを感じさせない工夫"
date: 2019-10-14 11:50:00 +0900
comments: true
categories:
  - Tech
tags:
  - Swift
  - Firebase
  - iOS App
  - 開発
  - SkeletonView
description: iOS アプリを Swift + Firebase(Cloud Firestore) で作りました。 Firebase からのデータロードの待ち時間をユーザーに感じさせないために SkeletonView を入れました。その導入メモです。
image: https://raw.githubusercontent.com/Juanpe/SkeletonView/master/Assets/header2.jpg
---
![FirebaseUI](https://raw.githubusercontent.com/Juanpe/SkeletonView/master/Assets/header2.jpg)

「フットサル」というフットサルを一緒に蹴るメンバーを募集したり簡単に参加できるアプリをリリースしました。

Swift + Firebase(Cloud Firestore) の構成です。

<div id="appreach-box" style="text-align: left; border: solid 0.5px #808000; border-radius: 8px; margin: 20px; padding: 5px; max-width: 420px;">
  <img id="appreach-image" src="https://is4-ssl.mzstatic.com/image/thumb/Purple113/v4/66/ec/a3/66eca3c8-3954-c9c1-586e-d50fdfa99705/source/512x512bb.jpg" alt="フットサル 募集" style="float: left; margin: 10px; width: 25%; max-width: 120px; border-radius: 10%; margin-right: 25px;">
  <div class="appreach-info" style="margin: 12px;">
    <div id="appreach-appname">フットサル</div>
    <div id="appreach-developer" style="font-size: 80%; display: inline-block;">
      開発元:
      <span id="appreach-developerurl">Players1st inc.</span>
    </div>
    <div id="appreach-price" style="font-size: 80%; display: inline-block;">無料</div>
    <div class="appreach-links" style="margin: 14px;">
      <div id="appreach-itunes-link" style="display: inline-block;">
        <a id="appreach-itunes" href="https://apps.apple.com/jp/app/%25E3%2583%2595%25E3%2583%2583%25E3%2583%2588%25E3%2582%25B5%25E3%2583%25AB/id1467175472?uo=4" target="_blank" rel="nofollow">
          <img src="https://nabettu.github.io/appreach/img/itune_ja.svg" style="height: 40px; width: 135px;">
        </a>
      </div>
    </div>
  </div>
</div>

メンバーを募集するイベントを誰でも作れるのですが、イベント一覧を Firebase から読み込むのに若干時間がかかり、空白のページで待ちが発生するような操作感になっていました。

ユーザーに待ちを感じさせないように SkeletonView を導入することにしました。

[https://github.com/Juanpe/SkeletonView](https://github.com/Juanpe/SkeletonView)

Facebookアプリにも使われていて、データが入る部分がグレーの背景色でまず描画されあとから読み込まれたデータが表示されると言う見え方をするので、ユーザーが真っ白な画面を見て待つ事がなくなり、感覚的に待たされている感じが減るというものです。


# 使い方

[Github の SkeletonView のページ](https://github.com/Juanpe/SkeletonView#-how-to-use)にありますが、対象の view に対して `showAnimatedSkeleton()` でスケルトン表示、`hideSkeleton()` で非表示にあります。

## サンプル

私のアプリの場合、イベント一覧の UITableViewController のページで使っています。

```swift
import UIKit
import CoreData
import Firebase
import SkeletonView

class EventsTableViewController: UITableViewController {

  (snip)

  func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
      // セルを取得する
      let cell = tableView.dequeueReusableCell(withIdentifier: "repeatCell", for: indexPath) as! CustomEventCell

      // SkeletonView show
      cell.dateLabel.showAnimatedSkeleton()
      cell.timeLabel.showAnimatedSkeleton()
      cell.courtLabel.showAnimatedSkeleton()
      cell.supportLabel.showAnimatedSkeleton()
      cell.numberLabel.showAnimatedSkeleton()
      cell.afterSupportLabel.showAnimatedSkeleton()

      let event = self.events[indexPath.row]
      let documentID = documentIDs[indexPath.row]
      // 申込人数 event.document.entries 数
      db.collection("entries").whereField("eventID", isEqualTo: documentID).getDocuments() { (querySnapshot, err) in
          if let err = err {
              print("Error getting documents: \(err)")
          } else {
              let d = DateFormatter()
              d.dateFormat = "M/d(E)"
              d.locale = Locale(identifier: "ja_JP")
              cell.dateLabel.text = d.string(from: event.start.dateValue())
              let t = DateFormatter()
              t.dateStyle = .none
              t.timeStyle = .short
              t.locale = Locale(identifier: "ja_JP")

              cell.timeLabel.text = "\(t.string(from: event.start.dateValue())) - \(t.string(from: event.end.dateValue()))"
              cell.courtLabel.text = event.courtName

              self.remainingCount = event.recruitment - querySnapshot!.documents.count
              if event.recruitment > querySnapshot!.documents.count {
                  cell.supportLabel.text = "あと"
                  cell.supportLabel.textColor = UIColor.black
                  cell.supportLabel.backgroundColor = UIColor.white
                  cell.numberLabel.text = String(self.remainingCount)
                  cell.afterSupportLabel.text = "名"
              } else {
                  cell.supportLabel.text = "開催決定"
                  cell.supportLabel.textColor = UIColor.black
                  cell.supportLabel.backgroundColor = UIColor.gray
                  cell.numberLabel.text = String(querySnapshot!.documents.count)
                  cell.afterSupportLabel.text = "名 申込"
              }

              // SkeletonView hide
              cell.dateLabel.hideSkeleton()
              cell.timeLabel.hideSkeleton()
              cell.courtLabel.hideSkeleton()
              cell.supportLabel.hideSkeleton()
              cell.numberLabel.hideSkeleton()
              cell.afterSupportLabel.hideSkeleton()
          }
      }
      return cell
  }

  (snip)

end
```

各セルを表示する時に `showAnimatedSkeleton()` して、Firebase からデータを受け取って view にデータを設定した後に `hideSkeleton()` しています。

このように簡単に使うことができます。

![sample](https://raw.githubusercontent.com/Juanpe/SkeletonView/master/Assets/gradient_animated.gif)

ローディング中のアニメーションなどのカスタマイズできます。

[https://github.com/Juanpe/SkeletonView#-custom-animations](https://github.com/Juanpe/SkeletonView#-custom-animations)

かんたんにユーザー体験のコントロールができるツールですね。
お役に立てれば幸いです😊

---

自社サービスのためだけに開発をしているので、iOSアプリ開発の受託はやっていませんが、新規事業・サービス立ち上げ・システム開発のコンサルティング・アドバイザーを行っています。
今までの実績は [こちら](/professional-career/) をご覧ください。

お仕事の相談いつでもウェルカムです。ご興味ある方は [こちら](/contact)からでもSNSのDMからでもお気軽にご相談ください。
