---
title: OSC 2009 Tokyo/Spring にいってきました
date: Fri, 20 Feb 2009 15:00:00 +0000
author: Hiro Fukami
layout: post
permalink: /2009/02/20/osc-2009-tokyo-spring/
tumblr_hirofukami_permalink:
  - http://hirofukami.tumblr.com/post/29609214446/osc-2009-tokyo-spring
  - http://hirofukami.tumblr.com/post/29609214446/osc-2009-tokyo-spring
  - http://hirofukami.tumblr.com/post/29609214446/osc-2009-tokyo-spring
  - http://hirofukami.tumblr.com/post/29609214446/osc-2009-tokyo-spring
tumblr_hirofukami_id:
  - 29609214446
  - 29609214446
  - 29609214446
  - 29609214446
original_post_id:
  - 277
  - 277
  - 277
  - 277
categories:
  - Business
tags:
  - event
  - OSC
  - report
---
<div class="section">
  <p>
    2/21(土)にオープンソースカンファレンス 2009 Tokyo/Spring に行ってきました。なにげに行くのは初めてだったかも。
  </p>
  
  <p>
    会場の雰囲気やスタッフの方のノリとかが Unix 的というか(分かりづらいか、、)古き良き IRI のエンジニアたちな感じがした。基本的には Unix/Linux をベースにシェルとかを駆使してサーバで色々できるようにしてしまおう！という感じのノリ。この明るくってでも濃ゆい感じが、web2.0がくる前のネットバブルちょっと後の時代とあっている。長らく職場でそんな感じはなくなっていたので、すごく懐かしい感じがした。
  </p>
  
  <p>
    聞けたのは3コマ。発表内容はそのうちオープンになると思うので、つたない抜粋メモ。
  </p>
  
  <p>
    <a name="seemore"></a>
  </p>
  
  <h4>
    はてなでの仮想化技術のあれこれ
  </h4>
  
  <p>
    speaker&#160;: 田中慎司さん はてな
  </p>
  
  <h5>
    はてなの紹介
  </h5>
  
  <ul>
    <li>
      2001年創業 最初は人力検索&#160;: 結構徐々にサービスを増やしてきたのだね
    </li>
    <li>
      1000万/月 月間PV10億くらい
    </li>
  </ul>
  
  <h5>
    Xen 使っている
  </h5>
  
  <ul>
    <li>
      準仮想化で</p> <ul>
        <li>
          2007 前半 CentOS5 系に移行 Xen試し始める
        </li>
        <li>
          その後サーバ管理ツール開発開始
        </li>
      </ul>
    </li>
    
    <li>
      仮想化サーバの構築ポリシーは同居させることでハードウエアリソースの利用率の向上をはかる <ul>
        <li>
          CPUが空いている web
        </li>
        <li>
          IOが空いている DB
        </li>
        <li>
          メモリがあいている キャッシュサーバ
        </li>
        <li>
          経験則的に人が割り当てている&#160;: 結構大変そう、いろいろなタイプが混在するのは混乱しないのかな？
        </li>
      </ul>
    </li>
  </ul>
  
  <h5>
    Xen運用
  </h5>
  
  <ul>
    <li>
      新規サーバ作成 install_xen.sh コマンド一発で1サーバ、7、8分で作れる パーテションきるところまで
    </li>
    <li>
      自前の運用ツール作った hostname, グラフ(CPU使用率？), ラック, CPU, memo容量, HDD容量, 備考
    </li>
    <li>
      自動制御 <ul>
        <li>
          30秒間DomUのhttpdにアクセスできないと強制再起動
        </li>
        <li>
          アプリケーションサーバなど突然落としてもいい場合に適用
        </li>
      </ul>
    </li>
    
    <li>
      完全仮想化はオーバーヘッドが大きすぎる
    </li>
    <li>
      デメリット ホストOS分のリソースを消費してしまう
    </li>
    <li>
      Xenのバグあり Dom0 再起動で解消
    </li>
    <li>
      DomUが探せるようにする DomUからDomOの情報は得られないので
    </li>
    <li>
      仮想化によって得られるもの 物理的なリソース制約からの解放
    </li>
  </ul>
  
  <h5>
    今後の構想
  </h5>
  
  <ul>
    <li>
      自動制御</p> <ul>
        <li>
          再起動,負荷に応じた増殖、移動、削減
        </li>
      </ul>
    </li>
    
    <li>
      他の仮想化技術の検討 <ul>
        <li>
          KVM 現在開発中 次期CentOSにのるらしい
        </li>
        <li>
          Xen以外も試したい
        </li>
      </ul>
    </li>
  </ul>
  
  <h4>
    CMS大集合！
  </h4>
  
  <p>
    speaker&#160;: いろいろな方々
  </p>
  
  <p>
    紹介のみの1コマだけ聞いた。
  </p>
  
  <h5>
    CMSパネラー自己紹介
  </h5>
  
  <ul>
    <li>
      SKIP ユーザグループ&#160;: 社内SNS
    </li>
    <li>
      Moodle研究会&#160;: e-learning 用 CMS
    </li>
    <li>
      Plone研究会 6年目&#160;: 汎用CMS FBIi,JETRO で使っているらしい <ul>
        <li>
          何カ国語でも対応できる # 要チェックか
        </li>
      </ul>
    </li>
    
    <li>
      OpenPNE&#160;: 機能はそれほど多くない 素人でもできるように <ul>
        <li>
          trac.openpne.jp で開発中のソース公開
        </li>
        <li>
          CSSを管理画面でいじれるようになった 見た目を変えられるようになった
        </li>
      </ul>
    </li>
    
    <li>
      Typo3 企業用のCMS&#160;: 機能が豊富
    </li>
    <li>
      XOOPS Cube&#160;: ゲームエンジンを入れている
    </li>
    <li>
      MyNETS&#160;: OpenPNE から派生 SNS運用PHPで書かれている
    </li>
    <li>
      MODx&#160;: 開発途中 初心者向け管理画面 デザインの自由度が高い 開発しやすい Ajaxを採用
    </li>
    <li>
      Geeklog Japanese&#160;: ブログを中心とした汎用CMS ドイツ人が作っている 日本では修正開発 多言語サイト <ul>
        <li>
          web アプリケーションフレームワークとして使える
        </li>
      </ul>
    </li>
    
    <li>
      concrete5&#160;: Usagi Project Ajaxをつかった もともとUS120万したパッケージ <ul>
        <li>
          管理画面なし その場で編集、マウスでぐりぐりする ブロックを言う考え方 日本語まだ
        </li>
      </ul>
    </li>
    
    <li>
      wordpress 渋谷 2.7から巨大なブログツールを作るようになる
    </li>
    <li>
      warp&#160;: wordpress,xoopsなどをwindows上で解凍して簡単インストール
    </li>
    <li>
      LinuxコンソーシアムCMS部会 ユーザとベンダをつなげる プローモーション役？
    </li>
    <li>
      Wkyインストーラ サーバに簡単にインストールできる
    </li>
    <li>
      <a href="http://www.ossj.jp" target="_blank">www.ossj.jp</a> どのくらい使っているのか、ソースが見える
    </li>
  </ul>
  
  <h4>
    仮想化友の会 「濃ぃ～い話」
  </h4>
  
  <p>
    2構成だて
  </p>
  
  <h5>
    1部目 kernbenchを自動化してみた
  </h5>
  
  <p>
    日本仮想化技術 大内 明さん
  </p>
  
  <ul>
    <li>
      AMD新しいチップを使って Xen 動く
    </li>
    <li>
      Linux カーネルコンパイル速度でベンチマーク測定
    </li>
    <li>
      HP BL465c でベンチマークとった <ul>
        <li>
          cpu amd opteron 2382
        </li>
        <li>
          memory 4GB
        </li>
      </ul>
    </li>
    
    <li>
      仮想マシン(完全仮想化) 1cpu
    </li>
    <li>
      -M オプション 負荷かかりすぎない
    </li>
    <li>
      1時間くらいまわす
    </li>
    <li>
      時間もったいないので、自動化した
    </li>
    <li>
      VMがDom0にリブート命令後自らシャットダウンするようにして動いた
    </li>
    <li>
      メールで通知される
    </li>
    <li>
      /etx/xen/auto に VM設定ファイルのシンボリックリンクを張る
    </li>
    <li>
      Dom0の /root に reboot.sh を置く
    </li>
    <li>
      done を受け取ったら shutdown
    </li>
    <li>
      一晩13回
    </li>
    <li>
      この資料はダウンロードできるようになる
    </li>
    <li>
      xm dom id ってのがある 番号が1で返ってくる
    </li>
    <li>
      ベンチマーク結果は AMD のサイトにアップされているはず
    </li>
  </ul>
  
  <h5>
    2部目 DTrace で xVM に濃いする話
  </h5>
  
  <ul>
    <li>
      OpenSolarisでXVM仕掛けて
    </li>
    <li>
      D言語 いろいろなところVMに入っている
    </li>
    <li>
      kernel の中のある関数のようだ
    </li>
    <li>
      Solarisにした意味はない
    </li>
    <li>
      xVM いわゆる Xen
    </li>
    <li>
      DTrace は内部構造を知るのに簡単なツール
    </li>
    <li>
      DTrace GUI もある
    </li>
    <li>
      コマンド dtrace -l -P fbt
    </li>
    <li>
      関数、コードバスを見る
    </li>
  </ul>
</div>