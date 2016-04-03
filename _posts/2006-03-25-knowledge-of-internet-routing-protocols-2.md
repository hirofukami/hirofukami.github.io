---
title: Knowledge of Internet Routing Protocols ～概要～
date: Sat, 25 Mar 2006 15:00:00 +0000
author: Hiro Fukami
layout: post
permalink: /2006/03/25/knowledge-of-internet-routing-protocols-2/
tumblr_hirofukami_permalink:
  - http://hirofukami.tumblr.com/post/29542447289/knowledge-of-internet-routing-protocols
  - http://hirofukami.tumblr.com/post/29542447289/knowledge-of-internet-routing-protocols
  - http://hirofukami.tumblr.com/post/29542447289/knowledge-of-internet-routing-protocols
  - http://hirofukami.tumblr.com/post/29542447289/knowledge-of-internet-routing-protocols
  - http://hirofukami.tumblr.com/post/29542447289/knowledge-of-internet-routing-protocols
tumblr_hirofukami_id:
  - 29542447289
  - 29542447289
  - 29542447289
  - 29542447289
  - 29542447289
original_post_id:
  - 574
  - 574
  - 574
  - 574
  - 574
categories:
  - Tech
tags:
  - know-how
  - Network
  - routing
---
<div class="section">
  <p>
    今後ルーティングプロトコルをガシガシ使うことがなくなると思うので、忘れていかないうちに自分のナレッジのメモ
  </p>
  
  <p>
    あまり運用上使わない部分は削ってしまうことにする。
  </p>
  
  <p>
    追加項目/ご指摘大歓迎
  </p>
  
  <p>
    現在執筆中・・・
  </p>
  
  <p>
    <a name="seemore"></a>
  </p>
  
  <h4>
    ルーティングプロトコル概要
  </h4>
  
  <dl>
    <dt>
      BGP
    </dt>
    
    <dd>
      AS間で用いるルーティングプロトコル、社外ISPと共通して用いる唯一用いるプロトコル
    </dd>
    
    <dt>
      IGP
    </dt>
    
    <dd>
      AS内部で用いるルーティングプロトコル
    </dd>
  </dl>
  
  <ul>
    <ul>
      <li>
        OSPF
      </li>
      <li>
        RIP
      </li>
      <li>
        static
      </li>
      <li>
        etc.
      </li>
    </ul>
  </ul>
  
  <dl>
    <dt>
      BGPとIGPの関係
    </dt>
    
    <dd>
      IGPのルーティングテーブルを用いてBGPセッションが成り立つ。具体的にはリカーシブルックアップのようにBGP経路のNexthopへ到達するためにはIGPのルーティングテーブルを参照することが通常であるため
    </dd>
  </dl>
  
  <h4>
    BGP
  </h4>
  
  <h5>
    eBGP
  </h5>
  
  <ul>
    <li>
      interface address 同士でのダイレクトな peering が通常
    </li>
    <li>
      AS 間には peering を行う小さなネットワークが存在 (private peer なら /30、IX peering なら /24)
    </li>
    <li>
      AS 間における BGP 運用責任はtransit 提供などのサービス提供形態以外は対等、損害賠償もない緩やかな覚書を結ぶ程度(覚書を結ばないISPもあり)
    </li>
  </ul>
  
  <h5>
    iBGP
  </h5>
  
  <ul>
    <li>
      loopback address にて peering</p> <ul>
        <li>
          バックボーン内は複数ネットワークにて冗長性を確保している場合が通常であり、interface address に依存しない peering が可能
        </li>
      </ul>
    </li>
  </ul>
  
  <h4>
    BGP のトラフィック制御方法
  </h4>
  
  <h5>
    上りトラフィック制御方法(origin AS から external 行き:出し)
  </h5>
  
  <ol>
    <li>
      Local Preference
    </li>
    <li>
      AS-path長
    </li>
    <li>
      MED (上書き)
    </li>
  </ol>
  
  <h5>
    下りトラフィック制御方法(external AS から origin AS 行き:入り)
  </h5>
  
  <ul>
    <li>
      Prepend (AS-path長)
    </li>
    <li>
      MED (external からの経路情報より)
    </li>
  </ul>
  
  <h5>
    その他の制御方法
  </h5>
  
  <ul>
    <li>
      community</p> <ul>
        <li>
          色づけ(それ自身では制御は行わない)、fliterで引っ掛けて LP/MED などの制御を実施する
        </li>
        <li>
          origin AS の経路、border ルータ、customer AS、external AS などを識別するために経路情報に付与する
        </li>
      </ul>
    </li>
    
    <li>
      eBGP multi hop <ul>
        <li>
          複数ネットワークをはさんだ状態でpeerを張る
        </li>
        <li>
          neighbor address への到達性は static を切って確保する必要がある
        </li>
        <li>
          同一ルータで複数回線接続する場合にはロードバランスが可能 <ul>
            <li>
              この時ループバックアドレス同士のpeerが通常
            </li>
          </ul>
        </li>
      </ul>
    </li>
    
    <li>
      multi path <ul>
        <li>
          best path 選択の際、最後のクラスターIDまで落ちずに手前の段階で同コストとして選定する
        </li>
        <li>
          複数本peerを張っている場合にはロードバランスが可能
        </li>
      </ul>
    </li>
  </ul>
</div>