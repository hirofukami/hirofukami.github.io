---
layout: post
title: "iOS アプリ開発 Firestore で緯度経度(GeoPoint) を扱う"
date: 2019-07-16 17:20:00 +0900
comments: true
categories:
  - Tech
tags:
  - Swift
  - Firebase
  - iOS App
  - 開発
  - Firestore
  - GeoPoint
  - 緯度経度
description: iOS アプリを Swift + Firebase(Cloud Firestore) で作りました。Firestore で緯度経度を扱うためのサンプルを残しておきます。
image: /images/2019/07/20190716_geo_pin.jpg
---
![FirebaseUI](/images/2019/07/20190716_geo_pin.jpg)

「フットサルメット」というフットサルを一緒に蹴るメンバーを募集したり簡単に参加できるアプリをリリースしました。

Swift + Firebase(Cloud Firestore) の構成です。

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

データ管理の部分は Cloud Firestore を使っています。

Firestore の扱えるデータ型の中に緯度、経度があって、1つのカラムで緯度経度が扱えるので、スマホアプリならではな使い方に便利そうです。
今回は場所の情報を扱ったので、この緯度経度のデータ型を使いました。

iOS アプリ開発で Firestore を使った日本語のブログ記事はまだ少なく、緯度経度データ型を使わず浮動小数点で緯度、経度それぞれカラムをもたせるような無理のあるサンプルはありました。

やっぱり純粋に用意されているデータ型に沿った使い方をしたほうが良いと思うので書いてみます。

開発環境は以下です。

* Xcode 10.1 (10B61)
* Development Target 12.1
* Swift 4.2
* FirebaseUI 6.2.1

# 緯度経度データ型を使う

緯度経度のデータ型は Firebase ならではなものです。データ型は GeoPoint といいます。

[Firestore を使う準備](https://firebase.google.com/docs/firestore/quickstart)にて、 `import Firebase` を記述しますが、この時点で緯度経度のデータ型である GeoPoint が使えるようになります。

GeoPoint 型の指定して初期化する場合。

```swift
var receivedPoint: GeoPoint = GeoPoint(latitude: 0.0, longitude: 0.0)

```

こんな感じ。

# サンプル

今回、Firestore での設定内容は以下とします。

コレクション名 : events

ドキュメントのデータ

* 場所名 placeName : String
* 緯度経度 point : GeoPoint
* 作成日時 createdAt : Timestamp
* 作成ユーザー名 ownerName : String
* 作成ユーザーID ownerID : String

作成ユーザーは Firebase Authentication で管理するユーザーです。ちなみにユーザーIDは自動発行されアルファベット(大文字小文字)と数字の混合なので String 扱いです。

# addDocument

ドキュメントを追加する場合のサンプルです。

```swift
import UIKit
import Firebase
import MapKit

var latitude: Double = 0.0
var longitude: Double = 0.0

// ログインユーザー情報
let currentUser = Auth.auth().currentUser!

var receivedCourtAddress = "" {
    didSet {
        CLGeocoder().geocodeAddressString(receivedCourtAddress) { placemarks, error in
            if let lat = placemarks?.first?.location?.coordinate.latitude {
                self.latitude = lat
            }
            if let lng = placemarks?.first?.location?.coordinate.longitude {
                self.longitude = lng
            }
        }
    }
}

    func buttonTapped(cell: ButtonCellOf<String>, row: ButtonRow) {

    let db = Firestore.firestore()
    var ref: DocumentReference? = nil

    ref = db.collection("events").addDocument(data: [
        "placeName": placeName,
        "point": GeoPoint(latitude: self.latitude, longitude: self.longitude),
        "createdAt": FieldValue.serverTimestamp(),
        "ownerName": currentUser.displayName ?? "",
        "ownerID": currentUser.uid
    ]) { err in
        if let err = err {
            let alert = UIAlertController(title: "エラー", message: "データが保存できませんでした。申し訳ございませんが、しばらくたってから入力してください。", preferredStyle: .alert)
            alert.addAction(UIAlertAction(title: "OK", style: .default, handler: nil))
            self.present(alert, animated: true, completion: nil)
            print("Error adding document: \(err)")
        } else {
            print("Document added with ID: \(ref!.documentID)")
            self.dismiss(animated: true, completion: nil)
        }
    }
}
```
住所(receivedCourtAddress)から CLGeocoder を使って緯度経度を出して、`latitude`, `longitude` に入れて events コレクションに対してドキュメントを追加しています。

ユーザーはログインしている前提。エラー処理はアラートを出すようにしています。

`"point": GeoPoint(latitude: self.latitude, longitude: self.longitude),` が GeoPoint を使った箇所です。

GeoPoint(latitude: *Double*, longitude: *Double*) のように設定すれば addDocument できます。

それぞれに指定する値は Double 型です。

## getDocument

ドキュメントを取得する場合のサンプルです。TableView に表示する前提です。

```swift
import UIKit
import CoreData
import Firebase

class ViewController: UIViewController, UITableViewDelegate, UITableViewDataSource{

    @IBOutlet weak var tableView: UITableView!

    var events: [Event] = [] {
        didSet {
            tableView.reloadData()
        }
    }
    var documentIDs: [String] = []

    let db = Firestore.firestore()

    override func viewDidLoad() {
        super.viewDidLoad()

        db.collection("events")
            .limit(to: 50)
            .getDocuments() { (querySnapshot, err) in
            if let err = err {
                print("Error getting documents: \(err)")
            } else {
                self.events = querySnapshot?.documents.map { Event(document: $0.data()) } ?? []
                self.documentIDs = querySnapshot?.documents.map { $0.documentID } ?? []
                print("document count: \(self.events.count)")
            }
        }
    }

    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
    }

    func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        return self.events.count
    }

    func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
        // セルを取得する
        let cell = tableView.dequeueReusableCell(withIdentifier: "repeatCell", for: indexPath) as! CustomEventCell

        let event = self.events[indexPath.row]
        let documentID = documentIDs[indexPath.row]
        return cell
    }

    // cellが押されたときに呼ばれる
    func tableView(_ tableView: UITableView, didSelectRowAt indexPath: IndexPath) {
        // 対象Rowの非選択処理
        tableView.deselectRow(at: indexPath, animated: true)

        var placeName: events[indexPath.row].placeName
        let latitude = events[indexPath.row].point.latitude
        let longitude = events[indexPath.row].point.longitude

        placeName = placeName.addingPercentEncoding(withAllowedCharacters: NSCharacterSet.urlQueryAllowed)!
        let urlString: String!
        if UIApplication.shared.canOpenURL(URL(string:"comgooglemaps://")!) {
            urlString = "comgooglemaps://?q=\(placeName)&center=\(latitude),\(longitude)&zoom=14&mapmode=standard"
        }
        else {
            urlString = "http://maps.apple.com/?q=\(placeName)&ll=\(latitude),\(longitude)&z=14"
        }
        if let url = URL(string: urlString) {
            UIApplication.shared.open(url)
        }
    }
}
```

point を GeoPoint 型で受け取って緯度経度にばらして、場所名(placeName)をキーワードに緯度経度を指定してマップアプリに遷移させるような処理です。

マップアプリは Google Maps が開けなければ Apple Maps が開くようになっています。

`event.point.latitude`、`event.point.longitude` のように簡単に緯度経度にバラけさせることができます。

# 使ってみてわかったこと

緯度経度を1つのカラムで管理できるのはスッキリして良い。
緯度経度にバラけさせるのもシンプルな書き方で良い。

# まだ試していないこと

getDocument する際に、

* 緯度経度順にソートするとどういう並びになるんだろう？北から南はわかりやすいが、東西は？
* where で任意の緯度経度から「緯度xx度以内、経度xxx度以上」とか指定する場合はどうやるんだろう？

今回は単純に緯度経度をデータとして保存して呼び出すだけだったのですが、ソートや where したい時の使い方がちょっと不明でした。(全般的に Google のドキュメントに載っている where のサンプルはシンプルすぎて、GeoPoint を使ったソート、where の方法は見つけられなかった。)

おわかりになる方は教えていただければです。

---

開発は自社サービスのために行うようにしているので、iOSアプリ開発の受託はやっていませんが、新規事業・システム開発の立ち上げのコンサルティング・アドバイザーを行っています。
今までの実績は [こちら](/professional-career/) をご覧ください。

お仕事の相談いつでもウェルカムです。ご興味ある方は [こちら](/contact)からでもSNSのDMからでもお気軽にご相談ください。
