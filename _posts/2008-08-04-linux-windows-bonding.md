---
title: Linux と Windows の bonding 切替り時間比較
date: Mon, 04 Aug 2008 15:00:00 +0000
author: Hiro Fukami
layout: post
permalink: /2008/08/04/linux-windows-bonding/
tumblr_hirofukami_permalink:
  - http://hirofukami.tumblr.com/post/29609186324/linux-windows-bonding
  - http://hirofukami.tumblr.com/post/29609186324/linux-windows-bonding
  - http://hirofukami.tumblr.com/post/29609186324/linux-windows-bonding
tumblr_hirofukami_id:
  - 29609186324
  - 29609186324
  - 29609186324
original_post_id:
  - 349
  - 349
  - 349
categories:
  - Tech
tags:
  - bonding
  - Network
  - Server
---
<div class="section">
  <p>
    メンテナンス中に試せたのでメモ。
  </p>
  
  <ul>
    <li>
      実験サーバ&#160;: HP ProLiant BL460c
    </li>
    <li>
      Windows&#160;: Windows 2003 Server SP1
    </li>
    <li>
      Linux&#160;: CentOS4.5(final)
    </li>
  </ul>
  
  <ul>
    <li>
      bonding 設定</p> <ul>
        <li>
          active/standby(primary/secondry) で preempt 方式
        </li>
        <li>
          Windows&#160;: OS としては機能がないので、hp から提供されている Utillity をインストール/設定する <ul>
            <li>
              <a href="http://h20000.www2.hp.com/bizsupport/TechSupport/SoftwareIndex.jsp?lang=en&cc=us&prodNameId=3176332&prodTypeId=15351&prodSeriesId=1842750&swLang=25&taskId=135&swEnvOID=1005#11395" target="_blank">ここ</a> から HP Network Configuration Utility for Windows Server 2003 を選んでダウンロードできる
            </li>
            <li>
              設定は パス評価タイマ間隔 3秒(最小値)
            </li>
          </ul>
        </li>
        
        <li>
          Linux&#160;: 前に書いた <a href="http://d.hatena.ne.jp/d_sea/20070919" target="_blank">この方法</a> を参考に miimon=100msec
        </li>
      </ul>
    </li>
  </ul>
  
  <ul>
    <li>
      実験方法</p> <ul>
        <li>
          各サーバの active 側NICにつながっているスイッチポート間をはずす
        </li>
        <li>
          その間サーバにて ping <a href="http://www.nikkei.co.jp" target="_blank">www.nikkei.co.jp</a> -t をしておく
        </li>
        <li>
          ping NG になってからどれくらいで OK になるかを計る
        </li>
      </ul>
    </li>
  </ul>
  
  <ul>
    <li>
      結果</p> <ul>
        <li>
          active 側を切ったとき primary => secondry</p> <ul>
            <li>
              Windows ping 5～7発前後でOKになる
            </li>
            <li>
              Linux ping 1発も落ちず
            </li>
          </ul>
        </li>
        
        <li>
          はずしたケーブルを元に戻す secondry => primary <ul>
            <li>
              Windows ping 10発前後でOKになる
            </li>
            <li>
              Linux ping 1発落ちる
            </li>
          </ul>
        </li>
      </ul>
    </li>
  </ul>
  
  <p>
    最初の切替りより、切り戻し時のほうが時間がかかる。
  </p>
  
  <p>
    ツールの性能次第になってしまったが、Windows は切り替わり時間がかかった。5秒以上の断はインターネットバックボーンから見るとまずい。
  </p>
  
  <p>
    それに引き換え、Linux のパフォーマンスはすばらしいなぁ。Unix/Linux のネットワーク周りの機能は本当に充実している。
  </p>
  
  <p>
    という意味ではサーバは Unix/Linux でといいたいが、Windows のほうが開発しやすいという意見が強いのが困ったところだ。
  </p>
</div>