---
title: YAMAHA SRT100 と YMS-VPN1(NATトラバーサルしたい)でVPN張る時の設定一工夫
date: Mon, 07 Apr 2008 15:00:00 +0000
author: Hiro Fukami
layout: post
permalink: /2008/04/07/yamaha-srt100-yms-vpn1-nat-vpn/
tumblr_hirofukami_permalink:
  - http://hirofukami.tumblr.com/post/29609169421/yamaha-srt100-yms-vpn1-nat-vpn
  - http://hirofukami.tumblr.com/post/29609169421/yamaha-srt100-yms-vpn1-nat-vpn
  - http://hirofukami.tumblr.com/post/29609169421/yamaha-srt100-yms-vpn1-nat-vpn
  - http://hirofukami.tumblr.com/post/29609169421/yamaha-srt100-yms-vpn1-nat-vpn
tumblr_hirofukami_id:
  - 29609169421
  - 29609169421
  - 29609169421
  - 29609169421
original_post_id:
  - 392
  - 392
  - 392
  - 392
categories:
  - Tech
tags:
  - NAT
  - Network
  - Router
  - Yamaha
---
<div class="section">
  <p>
    細かい部分だけど久々の Network ネタ。
  </p>
  
  <ul>
    <li>
      やりたいこと</p> <ul>
        <li>
          YAMAHA SRT100 へ YMS-VPN1(クライアントソフト)からいろんな場所からVPNを張りたい
        </li>
        <li>
          YMS-VPN1を入れたPCは一般的なブロードバンド環境下で使う <ul>
            <li>
              ブロードバンドルータで NAT した下につながるので、NATトラバーサルを使いたい
            </li>
          </ul>
        </li>
      </ul>
    </li>
  </ul>
  
  <p>
    <a href="http://netvolante.jp/download/manual/index.html#manual_srt100" target="_blank">マニュアル</a>上だとVPN部分の設定しか説明されていないけど、NATトラバーサルをしたい場合は設定が全然不足。
  </p>
  
  <h4>
    NATトラバーサルしたい時に必要なこと
  </h4>
  
  <ul>
    <li>
      静的IPマスカレードの設定</p> <ol>
        <li>
          [ルータ機能] &#8211; [NAT] &#8211; [インターフェイス] &#8211; [PPPoE] の設定を開く
        </li>
        <li>
          [IPマスカレード] の設定を開く
        </li>
        <li>
          最下部の [静的IPマスカレード] 内で以下項目を追加 <ul>
            <li>
              [内側のアドレス]:ルータ自身のLAN1側のip address、[プロトコル]:esp、[ポート番号]:なし
            </li>
            <li>
              [内側のアドレス]:ルータ自身のLAN1側のip address、[プロトコル]:udp、[ポート番号]:500
            </li>
            <li>
              [内側のアドレス]:ルータ自身のLAN1側のip address、[プロトコル]:udp、[ポート番号]:4500
            </li>
          </ul>
        </li>
      </ol>
    </li>
    
    <li>
      ポリシーフィルターの設定 <ol>
        <li>
          [セキュリティ機能] &#8211; [ポリシーフィルター] &#8211; [グループとユーザ定義サービスの一覧] の詳細を開く
        </li>
        <li>
          [サービスグループの設定] で [IPsec] の設定を開く
        </li>
        <li>
          [IKE]、[ESP] に続いて、[IPSEC-NAT-T] をプルダウンから選んで、確認/保存
        </li>
      </ol>
    </li>
  </ul>
  
  <p>
    この2つの設定をしないとNATトラバーサルするパケットを受け付けてくれない。
  </p>
  
  <p>
    マニュアルに載ってないので問い合わせないと絶対分からない。
  </p>
  
  <p>
    ヤマハのカスタマーサポートに問い合わせたらサクッと答えていた。マニュアルにに載ってないからよく答えているんだろうなぁ。でも、サポートの技術的スキルが高い印象でよかったなぁ。
  </p>
</div>