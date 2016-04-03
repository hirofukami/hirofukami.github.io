---
title: VMware Virtualization Forum 2008 に行ってきました
date: Tue, 18 Nov 2008 15:00:00 +0000
author: Hiro Fukami
layout: post
permalink: /2008/11/18/vmware-virtualization-forum-2008/
tumblr_hirofukami_permalink:
  - http://hirofukami.tumblr.com/post/29609201830/vmware-virtualization-forum-2008
  - http://hirofukami.tumblr.com/post/29609201830/vmware-virtualization-forum-2008
  - http://hirofukami.tumblr.com/post/29609201830/vmware-virtualization-forum-2008
tumblr_hirofukami_id:
  - 29609201830
  - 29609201830
  - 29609201830
original_post_id:
  - 311
  - 311
  - 311
categories:
  - Business
tags:
  - event
  - report
  - VMware
---
<div class="section">
  <p>
    VMwareがフォーラムをやるというので行ってきた。CTCなどのSIerが行うものも行ってみたけど、自分所のサービス紹介だけで技術的な話がほとんど聞かれなかったので、本家ならばもう少し技術的に参考になる話が聞けるかと思い行ってみた。
  </p>
  
  <p>
    Technical Deep のお題がついているものをいくつか事前登録しておいたが、会場に人が多くて立ち見もままならなかったので見るのをやめてしまったものが2つ。残念。。。しかし、事前登録しているのだから部屋の入口で申込書をチェックするくらいして欲しい。誰でも聞けてしまう状況だったし、そもそも定員に対して会場のキャパが合っていたのかも疑問。1セッション45分、資料配布なし、質問タイムなし。
  </p>
  
  <p>
    当日の資料は11/26に期間限定で<a href="http://info.vmware.com/content/jp_ev_dwn_VMVF2008" target="_blank">公開</a>された。
  </p>
  
  <p>
    以下、聞いたセッションのメモ。
  </p>
  
  <h4>
    VMware VDI Technical Deep Dive
  </h4>
  
  <dl>
    <dt>
      感想
    </dt>
    
    <dd>
      Technical Deep といっておきながら技術的な話は全くなかったのが残念。全ての時間を仮想サーバの想定スペックから物理的サーバ台数の算出に当てる。個人的にはネットワーク算出方法が24時間平均値を採用している時点でダウトだと思ったので(ピークで見ないと実際さばけなくなるので)、結果の値は信頼性のないものと思っている。
    </dd>
  </dl>
  
  <h5>
    アジェンダ
  </h5>
  
  <ul>
    <ul>
      <li>
        1 検討フェーズ
      </li>
      <li>
        2 設計構築フェーズ
      </li>
      <li>
        3 運用管理フェーズ
      </li>
    </ul>
  </ul>
  
  <h5>
    用語
  </h5>
  
  <ul>
    <li>
      VDI Virtual Desktop Infrastructure
    </li>
    <li>
      VDM Virtual Desktop Manager
    </li>
    <li>
      VDMクライアント クライアント端末にインストールするVDM接続用プログラム
    </li>
    <li>
      テンプレート 仮想デスクトップのテンプレート
    </li>
    <li>
      VDI GPO管理用テンプレート グループポリシーを定義するためのテンプレート
    </li>
  </ul>
  
  <h5>
    サイジングの前に
  </h5>
  
  <ul>
    <li>
      サイジングは成功の鍵
    </li>
    <li>
      サーバ統合とは異なるアプローチが必要
    </li>
  </ul>
  
  <h5>
    サイジングのポイント
  </h5>
  
  <ul>
    <li>
      サーバでも同じ
    </li>
    <li>
      CPU/memory/strage/network <ul>
        <li>
          CPU 一般平均値 2.79% しかしピーク時を考慮すること
        </li>
        <li>
          8VM/Core が推奨、それ以上はパフォーマンスが急に悪くなる
        </li>
        <li>
          市場で売れているのは クアッドコア2ソケットの8CPUもの
        </li>
      </ul>
    </li>
    
    <li>
      CPU <ul>
        <li>
          8コアなので 8*8=64VM/マシン
        </li>
        <li>
          1物理PCあたり平均130MHz消費とすると 64VM+オーバーヘッドで 1.68GHzくらい
        </li>
      </ul>
    </li>
    
    <li>
      メモリ <ul>
        <li>
          メモリシェアなし XP 512M*64VM 32GB
        </li>
        <li>
          メモリシェア機能あり 21.3GB 以上必要
        </li>
      </ul>
    </li>
    
    <li>
      ネットワーク <ul>
        <li>
          プレゼンテーション系ネットワーク VDMクライアントがある 画面情報飲み流れる
        </li>
        <li>
          バックエンド系ネットワーク バックエンド系サーバがある
        </li>
        <li>
          その中間に 仮想デスクトップ(ESX)がある
        </li>
      </ul>
    </li>
    
    <li>
      ライト 50kbps*64、へヴィー 100kbps*64、平均 2kbps*64 = 128kbps <ul>
        <li>
          # 24時間平均で考えている ピークを見なくてはNGでは？
        </li>
      </ul>
    </li>
    
    <li>
      ディスク <ul>
        <li>
          1LUNあたり32VM とする
        </li>
        <li>
          VMDK(OSとアプリケーション) 10GB*32=320GB
        </li>
        <li>
          vSwap、Suspend/Resume領域、VMログ、領域、などを足す。空き領域15%
        </li>
        <li>
          合計 820GB
        </li>
      </ul>
    </li>
    
    <li>
      サイジング例 <ul>
        <li>
          2Core 4GBメモリ で 500-750ユーザ数さばける
        </li>
      </ul>
    </li>
    
    <li>
      コスト 100台 <ul>
        <li>
          初期 1500:1389＄
        </li>
        <li>
          ランニング 196＄/年。588$/3年
        </li>
      </ul>
    </li>
  </ul>
  
  <h4>
    仮想環境の管理を自動化するVMwareの最新テクノロジー
  </h4>
  
  <dl>
    <dt>
      感想
    </dt>
    
    <dd>
      運用管理するためのツールの紹介で、結構デモしてくれたりプレゼンテーターの知識も深く面白かった。結局VMwareを入れたとたんに管理も、、となるとこれもまたVMwareで。となってメーカの戦略によって染まってしまうのだぁと思うと、ちょっと警戒してしまった。
    </dd>
  </dl>
  
  <ul>
    <li>
      自動化の最適は拡張性、標準化が抑えられれば</p> <ul>
        <li>
          # どうも目的が異なるような。。動機付けが弱い
        </li>
        <li>
          仮想化は自動化に向いている # 仮想化を使う目的があってその上での自動かなので理由付けにならない
        </li>
      </ul>
    </li>
  </ul>
  
  <h5>
    vCenter
  </h5>
  
  <ul>
    <li>
      オーケストレーションを支えるツール
    </li>
    <li>
      VI Client
    </li>
    <li>
      Web Access
    </li>
    <li>
      VI API
    </li>
  </ul>
  
  <h5>
    VI API
  </h5>
  
  <ul>
    <li>
      Webサービス SOAPとして実装 https/http 通信
    </li>
    <li>
      WSDKに記述
    </li>
    <li>
      利用方法 Toolkit がある&#160;: Toolkit for windows, Perl Toolkit, SDK <ul>
        <li>
          サンプルコードなどがある 作りやすくするためのもの
        </li>
      </ul>
    </li>
    
    <li>
      ESXホストの構成。仮想マシンのプロビジョニングと管理、VMFSの監視
    </li>
    <li>
      出来ないこと ハードウエア監視(CIMでできる)
    </li>
    <li>
      VI Client 単体で出来ること = VI API でできること
    </li>
  </ul>
  
  <h5>
    VI Toolkit for windows
  </h5>
  
  <ul>
    <li>
      スクリプトで記述する内容</p> <ul>
        <li>
          VI Toolkitが提供するスナップインを登録する
        </li>
        <li>
          VCに接続する
        </li>
        <li>
          管理オブジェクト
        </li>
      </ul>
    </li>
    
    <li>
      出来ること 繰り返し作業
    </li>
    <li>
      出来ないこと 一元管理、実行履歴/結果の管理
    </li>
  </ul>
  
  <h5>
    vCenter Orchestrator
  </h5>
  
  <ul>
    <li>
      汎用性の高いワークフロー定義/実行管理
    </li>
    <li>
      コンプライアンスと視認性の実現
    </li>
    <li>
      ワークフロー作成 ドラック&ドロップ 線でつなげる ユーザインタラクションも入れられる
    </li>
    <li>
      あらかじめ用意されたアクション 400以上用意、ないものは javascript を書けばOK
    </li>
    <li>
      web UI の生成 ユーザは生成された web UI 上で作業する
    </li>
    <li>
      プラグインとwebサービス外部連携 <ul>
        <li>
          SDK, WMI、ネットワークプロトコル telnet など
        </li>
      </ul>
    </li>
    
    <li>
      出来ること 自動化による作業運用コストの軽減
    </li>
    <li>
      開発することに近い
    </li>
  </ul>
  
  <h5>
    Lifecycle Manager
  </h5>
  
  <ul>
    <li>
      vCenter Orchestrator の上で作られたもの
    </li>
    <li>
      日本では未発売 USでは今年発売
    </li>
    <li>
      仮想マシンのライフサイクルを一元管理する
    </li>
    <li>
      承認作業もこの中で行う 承認されないものは作業できない仕組み
    </li>
    <li>
      ブラウザ上で行える
    </li>
  </ul>
</div>