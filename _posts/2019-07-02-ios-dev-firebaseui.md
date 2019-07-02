---
layout: post
title: "iOS アプリ開発 FirebaseUI を使ってみた"
date: 2019-07-02 10:20:00 +0900
comments: true
categories:
  - Tech
tags:
  - Swift
  - Firebase
  - iOS App
  - 開発
  - FirebaseUI
description: ひさびさに iOS アプリを Swift + Firebase で作りました。いくつか使ったものの中で FirebaseUI について開発メモを残しておきます。
image: /images/2019/07/20190702_firebaseui_welcome.jpg
---
![FirebaseUI](/images/2019/07/20190702_firebaseui_welcome.jpg)

iOS アプリ開発は以前 Titanium Mobile で作って3つほどリリースしましたが、Swift で作れるようになりたいと思い、5月、6月を使って開発しました。

「フットサルメット」というフットサルを一緒に蹴るメンバーを募集したり簡単に参加できるアプリをリリースしました。

Swift + Firebase の構成です。

<div id="appreach-box" style="text-align: left;">
  <img id="appreach-image" src="https://is4-ssl.mzstatic.com/image/thumb/Purple113/v4/66/ec/a3/66eca3c8-3954-c9c1-586e-d50fdfa99705/source/512x512bb.jpg" alt="フットサル 募集" style="float: left; margin: 10px; width: 25%; max-width: 120px; border-top-left-radius: 10%; border-top-right-radius: 10%; border-bottom-right-radius: 10%; border-bottom-left-radius: 10%;">
  <div class="appreach-info" style="margin: 10px;">
    <div id="appreach-appname">フットサル 募集</div>
    <div id="appreach-developer" style="font-size: 80%; display: inline-block;">
      開発元:
      <span id="appreach-developerurl">Players1st inc.</span>
    </div>
    <div id="appreach-price" style="font-size: 80%; display: inline-block;">無料</div>
    <div class="appreach-links" style="">
      <div id="appreach-itunes-link" style="display: inline-block;">
        <a id="appreach-itunes" href="https://apps.apple.com/jp/app/%25E3%2583%2595%25E3%2583%2583%25E3%2583%2588%25E3%2582%25B5%25E3%2583%25AB/id1467175472?uo=4" target="_blank" rel="nofollow">
          <img src="https://nabettu.github.io/appreach/img/itune_ja.svg" style="height: 40px; width: 135px;">
        </a>
      </div>
      <div id="appreach-gplay-link" style="display: inline-block;"></div>
    </div>
  </div>
  <div class="appreach-footer" style="margin-bottom: 10px; clear: left;"></div>
</div>

アプリが提供するソリューションの検証を兼ねているので、最初は機能を多く入れ込まずにシンプルにしましたが、それでもいくつかのライブラリーや Firebase の使い所の理解が必要で、思ったよりもいろいろかかってしまいました。

今回は、そんな中の一つ FirebaseUI の使い所について、開発メモ的に残しておきます。

開発環境は以下です。

* Xcode 10.1 (10B61)
* Development Target 12.1
* Swift 4.2
* FirebaseUI 6.2.1

# FirebaseUI とは

Firebase Authentication を API でのバックエンド的な処理でなく様々な認証方法に対して View も含めて処理してくれて、認証周りはまるごとおまかせできる大変便利なライブラリーです。

今回利用した認証方法は メール、Facebook、Google です。

Twitter も入れたかったのですが、FirebaseUI 6.2.1 では動きませんでした。この記事を書いている時点のバージョンは 8.0.2 なので、今なら動くかもしれない。(しかし、バージョン上がるのはやっ)

また、電話番号認証も試みましたが、APNs 通知(プッシュ通知)を事前に使えるようにする必要があるので、工数をかけすぎたくないために諦めました。

準備の仕方は[公式ドキュメント](https://firebase.google.com/docs/auth/ios/firebaseui)を見ていただければです。

が、メールリンク認証の部分は[メールリンク認証](https://firebase.google.com/docs/auth/ios/firebaseui#email_link_authentication)を使用する方法記載されていますが、よくあるメールアドレスとパスワードのログインであば、Firebase コンソールで Authentication のログイン方法でメール/パスワードを有効にするまででOK。

# コード

ドキュメントには実際どのファイルに記述すればよいか分かりづらいので、私の場合を紹介。

AppDelegate.swift に Firebase の初期化をしています。

```swift:AppDelegate.swift
class AppDelegate: UIResponder, UIApplicationDelegate {

    var window: UIWindow?

    override init() {
        super.init()
        // Firebase関連の機能を使う前に必要
        FirebaseApp.configure()
    }
   (snip)
```

実際にログイン・ログアウトの処理をしている箇所は以下。
レイアウト的に TableView を使いたかったので、 TableViewController の中でやっています。

余計な部分もありますが、ユーザー設定ページを作ろうとしている方にはそのまま流用できるのでは。

<script src="https://gist.github.com/d-sea/78810b5812486d28d2a172c0ff26fdfb.js"></script>

認証周りの View の処理がまるごと省略できて非常に便利です。

`func viewDidLoad()` の中で FirebaseUI の初期化をしています。

```
override func viewDidLoad() {
    super.viewDidLoad()
    self.authUI.delegate = self
    self.authUI.providers = providers
    (snip)
```

また、`Auth.auth().addStateDidChangeListener` を使ってログイン状態をリアルタイムに監視して `login_status` 変数に反映して、TableCell の表示内容に活かしています。

```
handle = Auth.auth().addStateDidChangeListener { (auth, user) in
    if user != nil {
        self.login_status = true
    } else {
        self.login_status = false
    }
}
```

一点うまく行かなかったのが、認証画面から離れる `didSignInWith` の部分の69行目で認証方法の View で認証がエラーになった際のメッセージを表示したかったのですが、この View 自体を非表示にするキャンセルボタンを押した際もこの部分での処理になるので、そこの分岐の仕方がわからず。結局、何も処理をしていない状態にしています。

```
//　認証画面から離れたときに呼ばれる（キャンセルボタン押下含む）
public func authUI(_ authUI: FUIAuth, didSignInWith user: User?, error: Error?){
    // 認証に成功した場合
    if error == nil {
        DispatchQueue.main.asyncAfter(deadline: .now() + 0.1) {
            let alert = UIAlertController(title: "ログインしました。", message: "", preferredStyle: .alert)
            alert.addAction(UIAlertAction(title: "OK", style: .default, handler: nil))
            self.present(alert, animated: true, completion: nil)
        }
    } else {
        // キャンセルボタンを押されたときは何も出さないようにしたい。
        // DispatchQueue.main.asyncAfter(deadline: .now() + 0.5) {
        //    let alert = UIAlertController(title: "認証失敗", message: "ログインに失敗しました。申し訳ございませんが、しばらくたってからし再度お試しください。", preferredStyle: .alert)
        //    alert.addAction(UIAlertAction(title: "OK", style: .default, handler: nil))
        //    self.present(alert, animated: true, completion: nil)
        // }
    }
}
```

# 使ってみてわかったこと

ドキュメントではわからなかったことをメモ。

## FirebaseUI 経由のアカウント作成は displayName がついてくる

メール、Facebook、Google の認証を試した結果、アカウント作成時には `displayName` が必ず入力されることがわかりました。

SNSアカウントを使っての認証はSNSアカウント情報から `displayName` を設定しますが、メールでアカウント作成する際は、フォームにパスワードと `Name` を入力させるようになっています。

![FirebaseUI](/images/2019/07/20190702_firebaseui_email_signup.jpg)

なので、ユーザー名を表示したい場合は、入力されている前提で `currentUser.displayName` が使えます。

1つ注意点は、Firebase コンソールのWeb画面から「ユーザーを追加」ボタンを押してユーザーを追加しようとすると、メールアドレスとパスワードの入力のみで、表示名はなく入力できません。
なので、Firebase コンソールでユーザー追加すると `displayName` が表示できないので注意です。
というか、実際使わないほうが良いと思っていて、iOSシュミレーターなので FirebaseUI 上でユーザー作成したほうが思わぬひっかかりが避けられて良いと思います。

アプリを作りたいと思う方の参考になればです😊

---

しかし、以前よりも格段に作りやすくなっていてアプリの時代になったなぁと思います。
数年前から MBaaS(Mobile Backend as a Service) はあって、いまいちはやらなかったのが Firebase として出てきたら割と使われる様になったのは時代の波なのかしら。

Cloud Firestore も今年に入って使われ始めているし、思ったよりも後に波が来ている印象。

開発は自社サービスのために行うようにしているので、iOSアプリ開発の受託はやっていませんが、新規事業・システム開発の立ち上げのコンサルティング・アドバイザーを行っています。
今までの実績は [こちら](/professional-career/) をご覧ください。

お仕事の相談いつでもウェルカムです。ご興味ある方は [こちら](/contact)からでもSNSのDMからでもお気軽にご相談ください。
