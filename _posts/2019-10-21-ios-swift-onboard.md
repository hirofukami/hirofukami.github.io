---
layout: post
title: "iOS アプリ開発 Onboard 初期起動時のガイド"
date: 2019-10-21 11:50:00 +0900
comments: true
categories:
  - Tech
tags:
  - Swift
  - Firebase
  - iOS App
  - 開発
  - Onboard
description: iOS アプリを Swift + Firebase(Cloud Firestore) で作りました。 アプリを最初に立ち上げた時にユーザーに説明をしたいために Onboard を入れました。その導入メモです。
image: /images/2019/10/20191021_screenshot_onboard.jpg
---
![Onboard](/images/2019/10/20191021_screenshot_onboard.jpg)

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

メンバーを募集するイベントを誰でも作れるのですが、ユーザーが説明なくいきなり使うには難しそうだったので、最初の起動時にどんな事ができるアプリなのか説明したいと思いました。

いくつかのオンボーディングのライブラリーはあるのですが、一番シンプルで導入コストが低そうな Onboard を導入することにしました。

[https://github.com/mamaral/Onboard](https://github.com/mamaral/Onboard)

# 使い方

[Github の Onboard のページ](https://github.com/mamaral/Onboard#usage)にありますが、各スライドを `OnboardingContentViewController` で定義して `OnboardingViewController` でまとめるといった感じで表示します。

各ページの文字フォントの大きさやスキップメニューを表示するかどうかなどカスタマイズもできます。

## サンプル

私のアプリの場合、アプリ起動時に表示したいので `AppDelegate.swift` に記述しています。

```swift
import UIKit
import Firebase
import FirebaseUI
import CoreData
import Onboard

@UIApplicationMain
class AppDelegate: UIResponder, UIApplicationDelegate {

    (snip)

    /// チュートリアル画面の初期設定
    func setOnBoard(_ application: UIApplication) {
        if true {
            let content1 = OnboardingContentViewController(
                title: "ようこそ、\nフットサルメットへ",
                body: "フットサルメットは、フットサルで一緒に蹴りたいメンバーを募集したり、参加できたりする、\nフットサルメンバー募集アプリです。\n\nシンプルなデザインで簡単に使えます。",
                image: UIImage(named: "BallWhite"),
                buttonText: "利用規約に同意して利用開始",
                action: nil
            )
            let content2 = OnboardingContentViewController(
                title: "イベントをつくる",
                body: "募集するにはイベントを作ります。\n\n日時やコート名を入力して登録すれば、すぐに募集開始です。",
                image: UIImage(named: "AddWhite"),
                buttonText: "",
                action: nil
            )
            let content3 = OnboardingContentViewController(
                title: "参加する",
                body: "ログインして参加するボタンを押すだけ。\n\nGoogle、Facebookアカウントで簡単にログインできます。",
                image: UIImage(named: "HandWhite"),
                buttonText: "",
                action: nil
            )
            let content4 = OnboardingContentViewController(
                title: "コート名をタップ",
                body: "マップアプリが開いて行き方が検索できます。",
                image: UIImage(named: "PlaceWhite"),
                buttonText: "",
                action: nil
            )
            let content5 = OnboardingContentViewController(
                title: "マイページ",
                body: "募集したイベントや参加申込したイベントを確認できます。",
                image: UIImage(named: "UserWhite"),
                buttonText: "はじめる",
                action: {
                    //遷移
                    let storyboard: UIStoryboard = UIStoryboard(name: "Main", bundle: nil)
                    let homeView = storyboard.instantiateViewController(withIdentifier: "TabBarViewController")as! TabBarViewController
                    self.window?.rootViewController = homeView
                    self.window?.makeKeyAndVisible()
                    //skipボタンを押したときに, 初回起動ではなくす
                    UserDefaults.standard.set(true, forKey: "firstLaunch")
                }
            )

            // フォントサイズ変更
            content1.titleLabel.font = content1.titleLabel.font.withSize(30)
            content1.bodyLabel.font = content1.bodyLabel.font.withSize(17)
            content2.titleLabel.font = content2.titleLabel.font.withSize(30)
            content2.bodyLabel.font = content2.bodyLabel.font.withSize(17)
            content3.titleLabel.font = content3.titleLabel.font.withSize(30)
            content3.bodyLabel.font = content3.bodyLabel.font.withSize(17)
            content4.titleLabel.font = content4.titleLabel.font.withSize(30)
            content4.bodyLabel.font = content4.bodyLabel.font.withSize(17)
            content5.titleLabel.font = content5.titleLabel.font.withSize(30)
            content5.bodyLabel.font = content5.bodyLabel.font.withSize(17)

            let bgImage = UIImage(named: "WalkThroughBackground")
            let vc = OnboardingViewController(backgroundImage: bgImage, contents: [content1, content2, content3, content4, content5])
            vc?.allowSkipping = true
            vc?.skipHandler = {
                print("skip")
                //遷移
                let storyboard: UIStoryboard = UIStoryboard(name: "Main", bundle: nil)
                let homeView = storyboard.instantiateViewController(withIdentifier: "TabBarViewController")as! TabBarViewController
                self.window?.rootViewController = homeView
                self.window?.makeKeyAndVisible()
                //skipボタンを押したときに, 初回起動ではなくす
                UserDefaults.standard.set(true, forKey: "firstLaunch")
            }
            // 最後のページが表示されるとき, skipボタンを消す
            content1.viewWillAppearBlock = {
                vc?.skipButton.isHidden = true
            }
            content2.viewWillAppearBlock = {
                vc?.skipButton.isHidden = false
            }
            content5.viewWillAppearBlock = {
                vc?.skipButton.isHidden = true
            }
            // 最後のページが消えるとき, skipボタンを表示(前ページに戻った場合のため)
            content5.viewDidDisappearBlock = {
                vc?.skipButton.isHidden = false
            }
            window?.rootViewController = vc
        }
    }

    (snip)

}
```

content5 を表示した時はスキップボタンを消す。遷移先アクションとして通常のアプリの起動に遷移するようにしています。

非常にシンプルに使うことができます。

![Onboard](/images/2019/10/20191021_screenshot_onboard.jpg)

これだけでオンボーディングが実現できるのは良いですね。
お役に立てれば幸いです😊

---

自社サービスのためだけに開発をしているので、iOSアプリ開発の受託はやっていませんが、新規事業・サービス立ち上げ・システム開発のコンサルティング・アドバイザーを行っています。
今までの実績は [こちら](/professional-career/) をご覧ください。

お仕事の相談いつでもウェルカムです。ご興味ある方は [こちら](/contact)からでもSNSのDMからでもお気軽にご相談ください。
