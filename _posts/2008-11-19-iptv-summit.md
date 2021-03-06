---
title: IPTV Summit に行ってきました
date: Wed, 19 Nov 2008 15:00:00 +0000
author: Hiro Fukami
layout: post
permalink: /2008/11/19/iptv-summit/
tumblr_hirofukami_permalink:
  - http://hirofukami.tumblr.com/post/29609202275/iptv-summit
  - http://hirofukami.tumblr.com/post/29609202275/iptv-summit
  - http://hirofukami.tumblr.com/post/29609202275/iptv-summit
tumblr_hirofukami_id:
  - 29609202275
  - 29609202275
  - 29609202275
original_post_id:
  - 310
  - 310
  - 310
categories:
  - Business
tags:
  - event
  - report
---
<div class="section">
  <p>
    今年6月にINTEROPと一緒にやっている<a href="http://www.imctokyo.jp/" target="_blank">IMC Tokyo</a> の関係で <a href="http://www.imctokyo.jp/iptv/" target="_blank">IPTV Summit</a> というものがあるらしく、IP系のメーカの話も聞けるようなので事前予約をして行ってみた。場所は幕張メッセ。やはり遠い。。。あまりしょっちゅう行く場所ではないな。
  </p>
  
  <p>
    IPTV Summit 自体は Inter BEE という放送系の展示会場内の端っこの方にブースを設けてその脇でクラスルームという話を聞ける場を設けていた。思ったよりもかなり小規模。聞いている人たちも20名行かないくらいか。
  </p>
  
  <p>
    シスコ、ジュニパー、モトローラの話を聞いたが、放送がIPに載れば彼らの培ってきた得意分野になるし、海外で実績を積んできているようだが日本はさぁこれから、というところか。各社の商品紹介ではなく、ジュニパーは海外のIPTV事情について、モトローラは日本の放送法、著作権法の法律関係の話がメイン。普段触れない側面なのでそれなりの成果はあった。
  </p>
  
  <p>
    以下、セッションを聞いたときのメモ。シスコの話はスピーカー曰く「ヨタ話」と言ってしまうほどひどいものだったのでカット。(個人的には相当怒りを感じてますが、ここに載せることでもないのでここまでにします。)
  </p>
  
  <h4>
    海外の事例に学ぶ収益性の高いIPTVインフラ構築
  </h4>
  
  <p>
    JuniperNetworks 佐宗大介
  </p>
  
  <ul>
    <li>
      世界的に展開しているNGN
    </li>
    <li>
      広帯域化の 現象
    </li>
    <li>
      売り上げ単価の 減少 <ul>
        <li>
          ブロードバンドARPU,携帯電話の1分あたりの利用額
        </li>
      </ul>
    </li>
    
    <li>
      海外大手通信事業者のチャレンジ <ul>
        <li>
          ベライゾン マネージドネットワークサービス
        </li>
        <li>
          AT&T IPTV
        </li>
      </ul>
    </li>
    
    <li>
      マーケットサイズ予測 169%増 2009年予測 <ul>
        <li>
          IPTV加入者予測も同様
        </li>
      </ul>
    </li>
    
    <li>
      IPTV市場参入の背景 <ul>
        <li>
          付加価値をつける
        </li>
        <li>
          IP化していろいろなものと融和性が高いだろう
        </li>
      </ul>
    </li>
    
    <li>
      VoDの役割 <ul>
        <li>
          VoDをやると売り上げが伸びる
        </li>
        <li>
          Concast 2005年に10億のVoDが視聴された 視聴率があがる
        </li>
        <li>
          解約率が 20-30%減少
        </li>
      </ul>
    </li>
  </ul>
  
  <h5>
    事例紹介
  </h5>
  
  <ul>
    <li>
      ケーブルTV事業者と通信事業者の戦い激しい
    </li>
    <li>
      ベライゾン FiOS 末端まで光 <ul>
        <li>
          $129 ARPU 伸びがすごい
        </li>
      </ul>
    </li>
    
    <li>
      AT&T <ul>
        <li>
          途中まで光そこからvDSL
        </li>
        <li>
          ネットワークを中心に
        </li>
        <li>
          予測では IPTV のトラフィックが2008年以降伸びるだろうと予測
        </li>
      </ul>
    </li>
    
    <li>
      イタリア Fastweb <ul>
        <li>
          750万世帯をカバー
        </li>
        <li>
          サービス VoD(5000タイトル)、TV を用意
        </li>
        <li>
          freeのチャネルもあり
        </li>
        <li>
          ゲームも手がける
        </li>
        <li>
          Juniper ERX-1440 入れている マルチキャストQoSかけている
        </li>
      </ul>
    </li>
    
    <li>
      Hong Kong 人口700万人 <ul>
        <li>
          チケット予約をテレビからできる
        </li>
        <li>
          宅配サービスも出来る
        </li>
        <li>
          4.5M 800k VoD がコンテンツサーバから ERX-1440 をへてマルチキャスト
        </li>
      </ul>
    </li>
    
    <li>
      IPTV配信するためには30-50くらいの要素が必要 <ul>
        <li>
          # ニコ動、Youtube ではその要素をできるだけ削減した形態なのでライトに出来ている
        </li>
      </ul>
    </li>
    
    <li>
      ネットワーク構成的に <ul>
        <li>
          edge ルータで管理している
        </li>
        <li>
          P2MPによるコンテンツ配信
        </li>
        <li>
          IPのルーティング上に MPLS のパスを張って映像用のルーティングさせていてる
        </li>
      </ul>
    </li>
    
    <li>
      P2MP パスの切替りが非常に速い
    </li>
    <li>
      edge にて階層化した QoS をかける
    </li>
    <li>
      例)3つ目のTVを付かないようにする(輻輳するので) <ul>
        <li>
          アプリケーションと加入者情報をリンクさせてポリシーを適用する
        </li>
      </ul>
    </li>
    
    <li>
      TOP100サービスプロバイダでのビジネス実績
    </li>
  </ul>
  
  <h4>
    IPTVビジネスを加速させるテレビジョン・オン・デマンドソリューション
  </h4>
  
  <p>
    モトローラ 石原　篤
  </p>
  
  <ul>
    <li>
      テレビジョン・オン・デマンド モトローラで提唱している
    </li>
    <li>
      電気通信役務利用放送法 自前で通信インフラを持たなくても放送可能になった
    </li>
    <li>
      ビービーケーブル(ソフトバンク)が2002年7月24日第一号登録 Yahoo!BB IP方式
    </li>
    <li>
      RF方式：ファイバを通じて現状のケーブルTVと同じような形態
    </li>
    <li>
      IPTVのメリット <ul>
        <li>
          既存IP網を使える
        </li>
        <li>
          放送業者は通信事業を委託することが出来る コンテンツ作りに特化できる
        </li>
        <li>
          IPとしての双方向性のメリット 他アプリケーションとの統合
        </li>
      </ul>
    </li>
    
    <li>
      IPTVの課題 著作権問題 <ul>
        <li>
          IPマルチキャスト方式は著作権法上は自動公衆送信として通信扱い</p> <ul>
            <li>
              NHKオンデマンドは権利者に個々に許諾を受けるようなことが必要になった
            </li>
          </ul>
        </li>
      </ul>
    </li>
    
    <li>
      竹中改革の功績 <ul>
        <li>
          2006/06/06-09/19&#160;2010年までに通信と放送の法体系の見直しを行うことを決めた
        </li>
        <li>
          新競争促進プログラムという 2009年12月国会に法案提出
        </li>
      </ul>
    </li>
    
    <li>
      NHKオンデマンド開始は大きな前進 法的な意味からも真の位置づけになるのはこれから2010年
    </li>
    <li>
      これまでのIPTV CATVに追いつくことが目標だった 受身的市長の習慣の延長
    </li>
    <li>
      これからのIPTV 好きな時間に好きな番組を広げる # オンデマンド性ということでしょ <ul>
        <li>
          # webで出来ていることと比べると真新しさがないなぁ
        </li>
      </ul>
    </li>
    
    <li>
      海外 <ul>
        <li>
          TimeWarner サーバで録画 好きなときに再生
        </li>
        <li>
          BBC オンデマンド再生
        </li>
        <li>
          StarHub オンデマンドテレビ
        </li>
      </ul>
    </li>
    
    <li>
      オンデマンド広告挿入 <ul>
        <li>
          早送りできないように強制的に見せる
        </li>
      </ul>
    </li>
    
    <li>
      Motorola B-1 サーバ <ul>
        <li>
          オンデマンド、リモートストレージDVR(RSDVR)は法律的にOKと判断された</p> <ul>
            <li>
              レコーダーがネットワーク上にある # クラウドぽいな
            </li>
          </ul>
        </li>
      </ul>
    </li>
    
    <li>
      B-1サーバ <ul>
        <li>
          128GBメモリ*10枚 メモリベースで早い
        </li>
      </ul>
    </li>
    
    <li>
      民法のオンデマンドは、、、広告があるから これから検討
    </li>
  </ul>
</div>