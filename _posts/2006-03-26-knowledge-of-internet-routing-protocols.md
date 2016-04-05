---
title: Knowledge of Internet Routing Protocols ～設計～
date: Sun, 26 Mar 2006 15:00:00 +0000
author: Hiro Fukami
layout: post
permalink: /2006/03/26/knowledge-of-internet-routing-protocols/
tumblr_hirofukami_permalink:
  - http://hirofukami.tumblr.com/post/29542447607/knowledge-of-internet-routing-protocols
  - http://hirofukami.tumblr.com/post/29542447607/knowledge-of-internet-routing-protocols
  - http://hirofukami.tumblr.com/post/29542447607/knowledge-of-internet-routing-protocols
  - http://hirofukami.tumblr.com/post/29542447607/knowledge-of-internet-routing-protocols
  - http://hirofukami.tumblr.com/post/29542447607/knowledge-of-internet-routing-protocols
tumblr_hirofukami_id:
  - 29542447607
  - 29542447607
  - 29542447607
  - 29542447607
  - 29542447607
original_post_id:
  - 573
  - 573
  - 573
  - 573
  - 573
categories:
  - Tech
tags:
  - Network
  - routing
---
<div class="section">
  <p>
    執筆中・・・
  </p>
  
  <p>
    <a name="seemore"></a>
  </p>
  
  <h4>
    設計
  </h4>
  
  <h4>
    BGP
  </h4>
  
  <ul>
    <li>
      iBGP ではフルメッシュ peering が必要だが、複数台存在する時点でルートリフレクタ(RR)を用いる</p> <ul>
        <li>
          peerの本数が減らすことが出来るのでルータの負荷軽減につながる
        </li>
        <li>
          複数 RR を置いている場合は同じ Cluster ID を付ける<span class="footnote"><a href="#f1" name="fn1" title='NANOG31 "BGP Techniques for Internet Service Providers" by Philip Smith' target="_blank">*1</a></span> <ul>
            <li>
              同一 Cluster ID は同一経路情報を破棄するため、RR 同士は差分の経路情報のみをやり取りできるので負荷軽減につながる？
            </li>
          </ul>
        </li>
      </ul>
    </li>
    
    <li>
      eBGP 受信経路への LP/MED は接続形態に沿って <ul>
        <li>
          LPで大まかに優先度を決めて、同じ LP のもので AS-path 長で比較したい時に MED で差をつけるなど、大まかな優先度は以下</p> <ol>
            <li>
              private peer
            </li>
            <li>
              public peer
            </li>
            <li>
              upstream
            </li>
          </ol>
        </li>
        
        <li>
          実際には複数本存在したりその間での優先度の決定やトラフィック量の調整のために prefix 単位で制御する場合もある
        </li>
      </ul>
    </li>
    
    <li>
      nexthop-self の活用 <ul>
        <li>
          iBGP に対して行う時は peering アドレスがどのルータに接続されている分かりづらいので、見やすくするために有効
        </li>
        <li>
          nexthop が peering address のままだとそこまでの到達性が BGP のみになるので、loopback address にして IGP で到達性を確保する意味もあり <ul>
            <li>
              実際は peering アドレスは OSPF に載せて、IGP としても保持しておく
            </li>
          </ul>
        </li>
        
        <li>
          eBGP では1ルータで複数 peering している時に peer 先に対して誤った nexthop を広報しないようにする意味で有効<span class="footnote"><a href="#f2" name="fn2" title='Internetweek2005 "ISPバックボーンネットワークにおける経路制御設計 ～実践編～" by 吉田友哉, NTT-Com' target="_blank">*2</a></span>
        </li>
      </ul>
    </li>
    
    <li>
      MD5 password の活用 <ul>
        <li>
          セキュリティの観点から全ての peer に適用するように推奨されている
        </li>
      </ul>
    </li>
  </ul>
  
  <h4>
    OSPF
  </h4>
  
  <ul>
    <li>
      Area0 にいくつまでルータが置けるか</p> <ul>
        <li>
          ルータのスペックにもよるが、厳密な推奨台数は無いためやってみてが多い</p> <ul>
            <li>
              1996年の文献では40～50台のルータが経験上、上限だといっている<span class="footnote"><a href="#f3" name="fn3" title='"OSPF設計ガイド" by Sam Halabi, Cisco Systems, Inc. 1994.04.01' target="_blank">*3</a></span>
            </li>
          </ul>
        </li>
        
        <li>
          LSDB がどこまで大きく出来るかにつながるので無駄な肥大化は避けたい
        </li>
        <li>
          地方拠点/edge 部分など役割と拡張性もみてAreaを切ることか可能なことが明確な部分にはには設ける
        </li>
      </ul>
    </li>
    
    <li>
      DR/BDR への意識 <ul>
        <li>
          複数ネットワークをルータが接続している場合、DR/BDRがかぶらないように、prorityで調整(実際には neighbor を張る順番にも依存するので必ずしも有効ではない)
        </li>
        <li>
          メンテナンスの少ない or 上位ルータを DR/BDR にする
        </li>
      </ul>
    </li>
    
    <li>
      cost <ul>
        <li>
          interface の帯域や NW構成の階層でシンプルに決める
        </li>
        <li>
          距離感をそのまま反映したい場合は type1 を用いる
        </li>
        <li>
          type 1/2 の混合は混乱しやすいので避けたい
        </li>
      </ul>
    </li>
    
    <li>
      default route の広報 <ul>
        <li>
          障害時を想定して上位のルータから default route を OSPF にて広報する
        </li>
        <li>
          広報するルータは全ての宛先がわかるという意味でフルルートを保持しているものにする
        </li>
      </ul>
    </li>
  </ul>
  
  <h4>
    全般的に、、
  </h4>
  
  <ul>
    <li>
      設計ポリシーはシンプルに、例外はなるべく少なく
    </li>
    <li>
      protocol distance/route preference の意識を常にもつ <ul>
        <li>
          メーカごとに異なるので注意する。OSPFとBGPの優先度など
        </li>
        <li>
          BGP経路に乗せていても広報できていない場合があり
        </li>
      </ul>
    </li>
    
    <li>
      障害時のトラフィック迂回時を想定する <ul>
        <li>
          このポイントが切れたらここに回るを把握する</p> <ul>
            <li>
              eBGP と IGP 両方の場合について
            </li>
          </ul>
        </li>
        
        <li>
          2次障害を加味してはきりが無いのである程度であきらめる
        </li>
      </ul>
    </li>
  </ul>
</div>

<div class="footnote">
  <p class="footnote">
    <a href="#fn1" name="f1" target="_blank">*1</a>：NANOG31 &#8220;BGP Techniques for Internet Service Providers&#8221; by Philip Smith
  </p>
  
  <p class="footnote">
    <a href="#fn2" name="f2" target="_blank">*2</a>：Internetweek2005 &#8220;ISPバックボーンネットワークにおける経路制御設計 ～実践編～&#8221; by 吉田友哉, NTT-Com
  </p>
  
  <p class="footnote">
    <a href="#fn3" name="f3" target="_blank">*3</a>：&#8221;OSPF設計ガイド&#8221; by Sam Halabi, Cisco Systems, Inc. 1994.04.01
  </p>
</div>