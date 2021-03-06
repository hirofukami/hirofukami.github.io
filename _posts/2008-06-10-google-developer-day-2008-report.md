---
title: Google Developer Day 2008 report
date: Tue, 10 Jun 2008 15:00:00 +0000
author: Hiro Fukami
layout: post
permalink: /2008/06/10/google-developer-day-2008-report/
tumblr_hirofukami_permalink:
  - http://hirofukami.tumblr.com/post/29609176448/google-developer-day-2008-report
  - http://hirofukami.tumblr.com/post/29609176448/google-developer-day-2008-report
  - http://hirofukami.tumblr.com/post/29609176448/google-developer-day-2008-report
  - http://hirofukami.tumblr.com/post/29609176448/google-developer-day-2008-report
tumblr_hirofukami_id:
  - 29609176448
  - 29609176448
  - 29609176448
  - 29609176448
original_post_id:
  - 374
  - 374
  - 374
  - 374
categories:
  - Business
tags:
  - event
  - report
---
<div class="section">
  <p>
    Googleの人たちに直接触れたことがなかったので、登録していってきた。目的はプレゼンテーションの中身と言うよりかはそれを話している人たちがどんなものなのか。の方が興味があったりして。
  </p>
  
  <p>
    3セッションほど聴けた。最後のエンジニアの日常の紹介では正に梅田望夫の本と同じ内容が紹介されていた。それ以外の部分で気になった点は、
  </p>
  
  <ul>
    <li>
      カルチャーを重んじる</p> <ul>
        <li>
          人の邪魔をしないとか新しい人を受け入れる など
        </li>
      </ul>
    </li>
    
    <li>
      情報を共有することを徹底する <ul>
        <li>
          グローバルにやっているのでリアルなコミュニケーションより基本メーリングリストベースなのだろう
        </li>
      </ul>
    </li>
    
    <li>
      リフレッシュする <ul>
        <li>
          結構日本人的には休むことに関してネガティブな感覚が根強いので、リフレッシュベタだと思う
        </li>
      </ul>
    </li>
    
    <li>
      完全にボトムアップ型 <ul>
        <li>
          プロジェクト参加も自分で手をあげて参加するそうな
        </li>
        <li>
          大企業に多い 言われてやる型 では成り立たない。逆に言えば言うだけの人はいらないと言うことだろう
        </li>
      </ul>
    </li>
  </ul>
  
  <p>
    <a name="seemore"></a>
  </p>
  
  <p>
    080610 Google Developer Day 2008 @パシフィコ横浜
  </p>
  
  <h4>
    KML
  </h4>
  
  <p>
    Mano Marks
  </p>
  
  <p>
    # 最初質問 KML使ったことある人 聞く
  </p>
  
  <p>
    # 話してほしいこと聞く
  </p>
  
  <h5>
    KML とは (<a href="http://code.google.com/intl/ja/apis/kml" target="_blank"><a href="http://code.google.com/intl/ja/apis/kml" target="_blank">http://code.google.com/intl/ja/apis/kml</a></a>)
  </h5>
  
  <ul>
    <li>
      Google Earth のために最初開発された位置情報などの静的ファイル形式
    </li>
    <li>
      Google Earth API として
    </li>
    <li>
      Google Earth 以外も連携可能になった
    </li>
    <li>
      チェックしてみてください <ul>
        <li>
          <a href="http://code.google.com/intl/ja/" target="_blank"><a href="http://code.google.com/intl/ja/" target="_blank">http://code.google.com/intl/ja/</a></a>
        </li>
        <li>
          もうすぐ日本語化される
        </li>
      </ul>
    </li>
  </ul>
  
  <h5>
    Google Earth を見せながらデモ
  </h5>
  
  <ul>
    <li>
      Google Earth 上で Place Mark 追加
    </li>
    <li>
      Google Earth 上でコピーして KMLファイルがテキストに貼れる <ul>
        <li>
          mark のスタイルとサイズ、高度、座標軸の指定など可能</p> <ul>
            <li>
              記述方法は HTML タグのように <StyleMap>,</StyleMap> のように囲む
            </li>
          </ul>
        </li>
      </ul>
    </li>
  </ul>
  
  <h5>
    Google Maps 上でも Place Mark がつけれる デモ
  </h5>
  
  <ul>
    <li>
      ある場所にマークをつける
    </li>
    <li>
      ファイルに保存すると Google Earth で見れる
    </li>
    <li>
      KML ファイルはローカルではなくリンクによってファイル指定が可能 <ul>
        <li>
          Google Maps 上のマークをコピーしてテキストエディタでペーストするとコードが貼れる
        </li>
        <li>
          逆に KML として Google Earth にペースト可能
        </li>
      </ul>
    </li>
  </ul>
  
  <h5>
    Google Earth にてデモ2
  </h5>
  
  <ul>
    <li>
      tokyo の年度ごとの人口変動を見せる
    </li>
    <li>
      3D 上で各区市 の人口が高さで表示されている <ul>
        <li>
          増加したところ赤、減少 青 出表示
        </li>
      </ul>
    </li>
    
    <li>
      動画再生のように 1955年から2015年予測を変動させた Community が作ってくれたKMLファイルらしい
    </li>
    <li>
      記述 <TimeSpan> <begin> <end> を必ず決める タイムスライダーを見せるため
    </li>
  </ul>
  
  <h5>
    QA その1
  </h5>
  
  <ul>
    <li>
      Q1 紀元前も可能？
    </li>
    <li>
      A1 TimeSpan は紀元前も可能
    </li>
    <li>
      Q2 Event も制御できますか
    </li>
    <li>
      A2 時間を制御している
    </li>
    <li>
      Q3 高さのマイナス 海の中とか扱うためにマイナスの値が使えるか
    </li>
    <li>
      A3 マイナスの値は取り扱いは出来ない KMLでは定義可能、Google Earth では出来ない
    </li>
  </ul>
  
  <h5>
    Google Maps と Google Earth を統合したもののデモ
  </h5>
  
  <ul>
    <li>
      Maps API に1行追加することで Earth API と連携できる</p> <ul>
        <li>
          # code.google.com からたどれるようだ
        </li>
      </ul>
    </li>
    
    <li>
      4つの Google Earth の画面をブラウザ上で操作できる
    </li>
  </ul>
  
  <p>
    # うまく Google Earth 表示できず
  </p>
  
  <h5>
    Google Earth デモ3
  </h5>
  
  <ul>
    <li>
      Places のチェックボックスをはずす
    </li>
    <li>
      サーバとの通信を削減できる
    </li>
    <li>
      デート相手を探す情報を表示させる
    </li>
    <li>
      <NetworkLink> している ローカルからでなくサーバへの問い合わせ？
    </li>
  </ul>
  
  <h5>
    QA その2
  </h5>
  
  <ul>
    <li>
      Q1 古い地図データは持ってこれるのか？
    </li>
    <li>
      A1 ある時点での地図データを使っている ユーザが地図データをオーバーレイすることは可能
    </li>
    <li>
      Q2 Google Earth API でも KML でもマーカー作れるけど どっちがいいの？
    </li>
    <li>
      A2 データ量が多い場合(マーカーの数が多い)は静的ファイルで格納したほうがいいKMLかXML、情報が少ない場合はダイナミックでもいい Earth API
    </li>
  </ul>
  
  <h4>
    Google Maps API for Flash
  </h4>
  
  <p>
    加藤定幸
  </p>
  
  <h5>
    Google Maps API for Flash
  </h5>
  
  <ul>
    <li>
      Flash 上で Google Maps を表示させる
    </li>
    <li>
      080515 にリリース すでに開発してくれているユーザいる
    </li>
  </ul>
  
  <h5>
    作成手順
  </h5>
  
  <ul>
    <li>
      ビルド環境の準備 Adbe SDK でもいいしフリーでもいい</p> <ul>
        <li>
          swf ファイルの生成
        </li>
      </ul>
    </li>
    
    <li>
      ActionScript ソースファイルを作成
    </li>
    <li>
      Maps API Key の取得 <ul>
        <li>
          API Key は AJAX用のキーがそのまま使える
        </li>
      </ul>
    </li>
  </ul>
  
  <h5>
    作成手順 詳細
  </h5>
  
  <ul>
    <li>
      Interface Library が必要 最新版推奨 verison1.4
    </li>
  </ul>
  
  <h5>
    記述方法
  </h5>
  
  <ul>
    <li>
      パッケージをimport する必要あり
    </li>
    <li>
      インベントリーリスナー <ul>
        <li>
          座標情報、maptype などを定義
        </li>
      </ul>
    </li>
    
    <li>
      Mapクラスを拡張
    </li>
    <li>
      MXML ファイルを作成 => コンパイル swf ファイル生成 => サーバにアップする
    </li>
  </ul>
  
  <h5>
    制御
  </h5>
  
  <ul>
    <li>
      イベント 4種類あり
    </li>
    <li>
      コントロール 4種類あり # Google Maps と同じイメージ <ul>
        <li>
          ControlBaseを拡張することでカスタマイズ可能
        </li>
      </ul>
    </li>
    
    <li>
      オーバーレイ 4種あり
    </li>
    <li>
      Poloyline:線をかく、Polygon:塗りつぶし
    </li>
    <li>
      画像表示可
    </li>
  </ul>
  
  <h5>
    デモ いろいろなアプリケーション紹介
  </h5>
  
  <ul>
    <li>
      マーカー位置をドラッグアンドドロップ移動させる
    </li>
    <li>
      移動後の拡大縮小時に地図の中央にマーカーが来るように制御
    </li>
    <li>
      ユーザ作成 飛行機操作 <ul>
        <li>
          飛行機が飛んでいるようなもの 3Dマウス？操作 ゲームコントローラーみたいなもの
        </li>
      </ul>
    </li>
    
    <li>
      プライオリティに応じてマーカーの表示非表示が可能 ズームすると現れるマーカーがある
    </li>
    <li>
      マーカーをクリックしたときに Flicker から画像を引っ張って表示する
    </li>
  </ul>
  
  <h5>
    まとめ MapsからいくつかAPIがあるので比較
  </h5>
  
  <ul>
    <li>
      Flash 版 視覚効果生かしたい
    </li>
    <li>
      Ajax 版 普通のwebサイトに埋め込むとき JavaScriptが得意な場合
    </li>
    <li>
      Static 版 静的なコンテンツが取得できればいい場合
    </li>
  </ul>
  
  <h5>
    最後に
  </h5>
  
  <ul>
    <li>
      日本のFlash製作能力はおそらく世界一
    </li>
    <li>
      ノウハウを生かしてアプリ作ってください
    </li>
    <li>
      開発チームは日本発のアプリケーション期待されている
    </li>
    <li>
      公開するときは英語のページも作ってください
    </li>
    <li>
      <a href="http://code.google.com/intl/ja/" target="_blank"><a href="http://code.google.com/intl/ja/" target="_blank">http://code.google.com/intl/ja/</a></a> 見てね
    </li>
  </ul>
  
  <h5>
    QA
  </h5>
  
  <ul>
    <li>
      Q1 携帯は？
    </li>
    <li>
      A1 Flash Lite 対応していない 今後も多分なし 互換性維持するのが難しい
    </li>
    <li>
      Q2 Javascript と Flash パフォーマンスどっちがいい？
    </li>
    <li>
      A2 評価したことない 前なら Flash だけどブラウザも向上しているのでよく分からない
    </li>
    <li>
      Q3 オフラインだと使える Javascprit だとだめだけど、KML みたいに出来るか？
    </li>
    <li>
      A3 Flash 版同じようにサーバ通信が発生しない ローカルファイルのアクセスはサポートしてない
    </li>
    <li>
      Q4 Airアプリは？
    </li>
    <li>
      A4 強い要望ある どうするかは未定
    </li>
    <li>
      Q5 Ajax 版で出来て Flash版で出来ないことは
    </li>
    <li>
      A5 Flash版ばかりやってたのでよく分からない。。
    </li>
    <li>
      Q6 カスタムアイコン 影つけられる？
    </li>
    <li>
      A6 やればできるけど、どうなっているか分からない
    </li>
  </ul>
  
  <h4>
    ソフトウエアエンジニアの日常
  </h4>
  
  <p>
    藤島さん
  </p>
  
  <h5>
    Googleのカルチャー
  </h5>
  
  <ul>
    <li>
      カルチャーを重視している会社
    </li>
    <li>
      すべてを明瞭に
    </li>
    <li>
      出来るだけすべての情報を共有する
    </li>
    <li>
      意思決定は命令ではなく社員の総意に基づいて判断する
    </li>
    <li>
      エピソード: 会社の移転先を決めるときに 社員の住んでいるところをみて移転先を決めた 合理的に決める
    </li>
    <li>
      他人の仕事がしやすいように
    </li>
    <li>
      20%の時間を使ってグループ横断活動で支援
    </li>
    <li>
      メンター: 新しいエンジニアについてヘルプする
    </li>
    <li>
      個人攻撃は起こらない
    </li>
    <li>
      自ら始める
    </li>
    <li>
      問題があれば 自ら直す
    </li>
    <li>
      完璧になるのを待つのではなく試してみる
    </li>
    <li>
      結果には柔軟かつ迅速に対処する
    </li>
    <li>
      今使えるものを使って目標を実現する
    </li>
    <li>
      安く効率的に実現する方法を考える
    </li>
    <li>
      ex.) 非効率な作業を見つけたらツールを作る
    </li>
    <li>
      周囲の人を楽しませる
    </li>
    <li>
      地味な仕事、泥臭い作業にも光を当てる
    </li>
    <li>
      世界を変えられることを忘れない
    </li>
  </ul>
  
  <h5>
    ソフトウエア開発
  </h5>
  
  <ul>
    <li>
      世界に20カ国以上に50箇所オフィスある
    </li>
    <li>
      すべてのオフィスは対等 自分の近くのオフィスに参加する
    </li>
    <li>
      コミュニケーション 1箇所のオフィスで完結するものなし <ul>
        <li>
          wiki、チャット あらゆるものを使う
        </li>
      </ul>
    </li>
    
    <li>
      アイデアがひらめいたら 20% プロジェクトとして開始する ボトムアップ <ul>
        <li>
          認められたらメインプロジェクトになる
        </li>
      </ul>
    </li>
    
    <li>
      少人数 変更も柔軟にできる
    </li>
    <li>
      ソフトウエアエンジニアが開発のすべてに携わる <ul>
        <li>
          アイデアから運用保守まで すべてのエンジニアがかかっている
        </li>
      </ul>
    </li>
    
    <li>
      必要な文書を作る、不要なものは作らない
    </li>
    <li>
      百文書は1デモにしかず # まず作って説明する
    </li>
  </ul>
  
  <h5>
    コーディング
  </h5>
  
  <ul>
    <li>
      言語 C++、Java, Python, Javascript
    </li>
    <li>
      読みやすいコード スタイルは全社で統一 スタイルにあったコードをかけるか社内認定がある
    </li>
    <li>
      1つのソースツリーを全世界で共通
    </li>
  </ul>
  
  <h5>
    採用活動
  </h5>
  
  <ul>
    <li>
      開発以外の重要な例外 採用活動への協力</p> <ul>
        <li>
          社風に会うか お互い Happly になるか
        </li>
      </ul>
    </li>
    
    <li>
      一緒に働きたいと思うか
    </li>
    <li>
      結構エネルギーと時間を使っている
    </li>
  </ul>
  
  <h5>
    業績評価
  </h5>
  
  <ul>
    <li>
      一緒に仕事した人の業績評価する 四半期ごとに目標の設定と評価している
    </li>
    <li>
      メリット 目立つだけでなく重要な地味なものも評価される
    </li>
    <li>
      社員間の信頼関係が前提
    </li>
  </ul>
  
  <h5>
    遊び
  </h5>
  
  <ul>
    <li>
      遊びと仕事は両立する
    </li>
    <li>
      チームでリフレッシュ
    </li>
  </ul>
  
  <h5>
    スピーカーの例 紹介
  </h5>
  
  <ul>
    <li>
      入社動機 優秀な中でやってみたい
    </li>
    <li>
      優秀だけでなく人間的魅力ある人が多い
    </li>
    <li>
      意見に対して誰が言ったかではなく、正論か通りやすい
    </li>
    <li>
      動きが早い すぐ世界に出て行く どんどん改善されていく
    </li>
    <li>
      エンジニアが 尊重/信頼されている
    </li>
    <li>
      良いソフトウエア開発のために泥臭いこともやる
    </li>
  </ul>
  
  <h5>
    1日の過ごし方紹介
  </h5>
  
  <ul>
    <li>
      10:00 メール USはまだ起きているかも
    </li>
    <li>
      11:00 コードレビュー
    </li>
    <li>
      12:00 昼飯
    </li>
    <li>
      13:00 コーティングに没頭
    </li>
    <li>
      16:00 tech talk を聞く 他のプロジェクト、技術話題聞く
    </li>
    <li>
      17:00 メールチェック後、コーディング
    </li>
    <li>
      18:00 夕食
    </li>
    <li>
      19:00 三度、コーディング
    </li>
  </ul>
  
  <h5>
    心がけていること
  </h5>
  
  <ul>
    <li>
      分かりやすいコードを書く
    </li>
    <li>
      他人のコードを尊重する <ul>
        <li>
          必要ならリファクタリング
        </li>
      </ul>
    </li>
    
    <li>
      正確かつ簡潔なコミュニケーションをする <ul>
        <li>
          文化が異なる、英語を使っている
        </li>
      </ul>
    </li>
    
    <li>
      仕事以外も目に向ける <ul>
        <li>
          TechTalk(他プロジェクトの紹介、技術紹介など) などに参加する
        </li>
      </ul>
    </li>
    
    <li>
      良き仲間と思われているかどうか
    </li>
    <li>
      ボトムアップの会社なので指示待ち、承認待ちをしていては進まない
    </li>
  </ul>
  
  <h5>
    QA
  </h5>
  
  <ul>
    <li>
      Q1 意思決定の承認フローは？
    </li>
    <li>
      A1 プロセス、承認を固める文化ではない
    </li>
    <li>
      Q2&#160;20%プロジェクトからメインサービスになるにはどうなっていく？
    </li>
    <li>
      A2 やっている人たちの熱意 デモの力の入れよう いろんな人に話す。最終的にはマネージャの承認を得る
    </li>
    <li>
      Q3 テスト環境は？
    </li>
    <li>
      A3 詳しくは言えないが自分のテスト環境は自分が作る <ul>
        <li>
          自分が直したことで解消されたことを証明する 担当チームへレビューする
        </li>
      </ul>
    </li>
  </ul>
</div>