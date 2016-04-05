---
title: Titanium Developerで開発したiPhoneアプリをAppStoreに登録する方法
date: Sun, 16 Oct 2011 22:25:06 +0000
author: Hiro Fukami
layout: post
permalink: /2011/10/16/titanium-developer-iphone-appstore/
tumblr_hirofukami_permalink:
  - http://hirofukami.tumblr.com/post/29678367918/titanium-developer-iphone-appstore
  - http://hirofukami.tumblr.com/post/29678367918/titanium-developer-iphone-appstore
tumblr_hirofukami_id:
  - 29678367918
  - 29678367918
original_post_id:
  - 172
  - 172
categories:
  - Tech
tags:
  - dev
  - release
  - tips
  - titanium
---
<div class="section">
  <p>
    Titanium Developer はもう古くて TitaniumStudio だとは思うのだけど、一応メモしておいたので公開。
  </p>
  
  <p>
    事前に以下のことが終わっていること
  </p>
  
  <ul>
    <li>
      実機でのテストが終わっていること
    </li>
    <li>
      iOS Dev Center, iTunes Connectへのログインが出来ること <ul>
        <li>
          ￥10,800の支払いがが終わっていること
        </li>
      </ul>
    </li>
  </ul>
  
  <p>
    参考にしたページ: <a href="http://blog.livedoor.jp/tattyamm/archives/2957285.html" target="_blank"><a href="http://blog.livedoor.jp/tattyamm/archives/2957285.html" target="_blank">http://blog.livedoor.jp/tattyamm/archives/2957285.html</a></a>
  </p>
  
  <h5>
    1. Distribution Certificate作成用CSRファイルの作成
  </h5>
  
  <ol>
    <li>
      手元のMacのキーチェーンアクセスから作成する
    </li>
    <li>
      Distribution用フォルダに保存する
    </li>
    <li>
      ファイル名: CertificateSigningRequest.certSigningRequest
    </li>
  </ol>
  
  <h5>
    2. iOS Provisioning PortalにてDistribution用Certificateを作りダウンロード
  </h5>
  
  <ol>
    <li>
      ローカルに保存したCSRファイルをアップロードする
    </li>
    <li>
      Distribution用フォルダに保存する
    </li>
    <li>
      ファイル名: distribution_identity.cer
    </li>
    <li>
      同じページより、AppleWWDRCA.cerもダウンロードする
    </li>
    <li>
      2つのファイルをダブルクリックして実行し、キーチェーンアクセスに登録する
    </li>
  </ol>
  
  <h5>
    3. Distribution用Provisioningファイルを作成する
  </h5>
  
  <ol>
    <li>
      developmentと同じprofile nameは使えないので変える</p> <ul>
        <li>
          どうやら大文字小文字区別は付けていないようだ
        </li>
      </ul>
    </li>
    
    <li>
      Distribution用ファルダに保存する <ul>
        <li>
          参考 ファイル名: dmatchat_distribution.mobileprovision
        </li>
      </ul>
    </li>
    
    <li>
      ダウンロードしたファイルを実行し、Xcode上に登録する
    </li>
  </ol>
  
  <h5>
    4. Titaniumでパッケージを作る
  </h5>
  
  <p>
    <a href="http://f.hatena.ne.jp/d_sea/20111018191153" class="hatena-fotolife" target="_blank"><img src="http://cdn-ak.f.st-hatena.com/images/fotolife/d/d_sea/20111018/20111018191153.png?w=830" alt="f:id:d_sea:20111018191153p:image" title="f:id:d_sea:20111018191153p:image" class="hatena-fotolife" data-recalc-dims="1" /></a>
  </p>
  
  <p>
    ここからの参考: <a href="http://webtech-walker.com/archive/2011/02/22130853.html" target="_blank"><a href="http://webtech-walker.com/archive/2011/02/22130853.html" target="_blank">http://webtech-walker.com/archive/2011/02/22130853.html</a></a>
  </p>
  
  <ol>
    <li>
      Test & Package &#8211; Distributeより、Distribution用Provisioningファイルを指定する
    </li>
    <li>
      Select Distribution Locationで適当なフォルダを指定する
    </li>
    <li>
      Provisioningファイルを置いたDistribution用フォルダにした
    </li>
    <li>
      Packageボタンを押す、エラーがなければXcodeのArchivesに表示される
    </li>
  </ol>
  
  <h5>
    5. iTunes Connect にてアプリ登録する
  </h5>
  
  <p>
    ここからの参考 <a href="http://blog.livedoor.jp/tattyamm/archives/1177705.html" target="_blank"><a href="http://blog.livedoor.jp/tattyamm/archives/1177705.html" target="_blank">http://blog.livedoor.jp/tattyamm/archives/1177705.html</a></a>
  </p>
  
  <ol>
    <li>
      iTunes Connectにログイン
    </li>
    <li>
      Manage Your Applications &#8211; Add New App
    </li>
    <li>
      Ready to Upload Binary を押して No を選ぶ
    </li>
    <li>
      statusがWaiting For UploadになったらOK
    </li>
  </ol>
  
  <h5>
    6. XcodeのArchivesからSubmitする
  </h5>
  
  <p>
    <a href="http://f.hatena.ne.jp/d_sea/20111018191151" class="hatena-fotolife" target="_blank"><img src="http://cdn-ak.f.st-hatena.com/images/fotolife/d/d_sea/20111018/20111018191151.png?w=830" alt="f:id:d_sea:20111018191151p:image" title="f:id:d_sea:20111018191151p:image" class="hatena-fotolife" data-recalc-dims="1" /></a>
  </p>
  
  <ol>
    <li>
      該当のアプリを選択して、Submitボタンを押す
    </li>
    <li>
      iTunes Connectへのログイン情報を入れる
    </li>
    <li>
      証明書をdistributionを選択をする
    </li>
    <li>
      しばらく待つと完了
    </li>
  </ol>
  
  <h5>
    7. iTunes Connect上のステータスを確認
  </h5>
  
  <ol>
    <li>
      Waiting For Review になっていればOK
    </li>
    <li>
      完了
    </li>
  </ol>
</div>