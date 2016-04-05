---
title: Titanium Mobile で開発した iPhone アプリに AdMob を導入する
date: Mon, 23 Jan 2012 15:24:48 +0000
author: Hiro Fukami
layout: post
permalink: /2012/01/23/titanium-mobile-iphone-admob/
tumblr_hirofukami_permalink:
  - http://hirofukami.tumblr.com/post/29678369853/titanium-mobile-iphone-admob
  - http://hirofukami.tumblr.com/post/29678369853/titanium-mobile-iphone-admob
tumblr_hirofukami_id:
  - 29678369853
  - 29678369853
original_post_id:
  - 168
  - 168
categories:
  - Tech
tags:
  - AD
  - AdMob
  - apps
  - dev
  - titanium
---
<div class="section">
  <p>
    バージョンアップ開発中のすき間iPhoneアプリ<a href="http://www.shakesoul.net/dmatchat" target="_blank">DM@chat</a>にAdmobを表示したくて調べたけれど、あまりきれいにまとまった手順がなかったので、自分のためにも残しておく。
  </p>
  
  <p>
    Titanium Mobile の提供元である Appcelerator から AdMob 用のモジュールも提供されたので、それを iPhone アプリに導入するための手順です。
  </p>
  
  <p>
    Appcelerator Developer Blogより<a href="http://developer.appcelerator.com/blog/2011/07/building-apps-with-modules.html" target="_blank">modules提供のお知らせのポスト</a>だと手順がわかりづらかったので、それより新しめの<a href="http://developer.appcelerator.com/blog/2011/09/admob-for-android-module-tutorial-and-code.html" target="_blank">Android導入用のポスト</a>の動画を参考にさせてもらった。
  </p>
  
  <h5>
    1. Appcelerator の github から AdMob modules をダウンロードする
  </h5>
  
  <p>
    いくつかのmodulesが<a href="https://github.com/appcelerator/titanium_modules" target="_blank">githubで公開されている</a>。
  </p>
  
  <ol>
    <li>
      今回必要なのはiPhone用のAdMobモジュールなので、<a href="https://github.com/appcelerator/titanium_modules/tree/master/admob/mobile/ios" target="_blank">この階層</a>(admob/mobile/ios)まで移動する。
    </li>
    <li>
      このディレクトリ内にある、「ti.admob-iphone-***.zip」というファイル名を見つける。 <ul>
        <li>
          自分が確認したときは ti.admob-iphone-1.0.zip と ti.admob-iphone-1.1.zip が置かれていた。
        </li>
        <li>
          このファイルはビルドされた時に生成されるべきzipファイルだと思うけど、すでにが置かれていたのでビルドの必要はなかった。
        </li>
      </ul>
    </li>
    
    <li>
      zipファイル名をクリックする。 <ul>
        <li>
          今回の場合、「ti.admob-iphone-1.1.zip」にした。
        </li>
      </ul>
    </li>
    
    <li>
      次に表示された画面にて「raw」リンクをクリックする。ダウンロードできるので適当な場所に保存する。
    </li>
  </ol>
  
  <h5>
    2. Titanium Mobile プロジェクト内へ配置する
  </h5>
  
  <ol>
    <li>
      ダウンロードしたzipファイルをプロジェクトディレクトリ直下(Resourcesやtiapp.xmlと同じ階層)に移動する。
    </li>
    <li>
      zipファイルを開き解凍する
    </li>
    <li>
      modulesディレクトリが作成される。 <ul>
        <li>
          中身は今回の場合、iphone/ti.admob/1.1/&#8230; のようになっている。
        </li>
      </ul>
    </li>
    
    <li>
      不要になったzipファイルを削除する。
    </li>
  </ol>
  
  <p>
    以下のように、Resourcesディレクトリとtiapp.xmlファイルと同じ階層に「modules」ディレクトリが置かれればOK。
  </p>
  
  <p>
    <a href="http://f.hatena.ne.jp/d_sea/20120124112355" class="hatena-fotolife" target="_blank"><img src="http://cdn-ak.f.st-hatena.com/images/fotolife/d/d_sea/20120124/20120124112355.png?w=830" alt="f:id:d_sea:20120124112355p:image" title="f:id:d_sea:20120124112355p:image" class="hatena-fotolife" data-recalc-dims="1" /></a>
  </p>
  
  <h5>
    3. tiapp.xml を編集する
  </h5>
  
  <ol>
    <li>
      tiapp.xml を適当なエディタソフトで開く。
    </li>
    <li>
      <modules></modules>の部分記述を変える。 <ul>
        <li>
          今回はバージョン1.1なので、以下のように記述した。
        </li>
      </ul>
    </li>
  </ol>
  
  <pre class="syntax-highlight">

<span class="synIdentifier">&lt;modules&gt;</span>

<span class="synIdentifier">&lt;module </span><span class="synType">version</span>=<span class="synConstant">"1.1"</span><span class="synIdentifier">&gt;</span>ti.admob<span class="synIdentifier">&lt;/module&gt;</span>

<span class="synIdentifier">&lt;/modules&gt;</span>

</pre>
  
  <h5>
    4. (一度動作確認したければ、)example/app.js で動かしてみる。
  </h5>
  
  <ol>
    <li>
      プロジェクト内の app.js を適当な名前にリネームして、modules/iphone/ti.admob/1.1/example にある app.js をコピーしてくる。
    </li>
    <li>
      Titanium Studioのメニューより、該当のプロジェクトに対して、Project > Clean を実施する。 <ul>
        <li>
          すでに作成済みなプロジェクトに AdMob を導入しようとしている場合は実施しないと、シミュレータ起動時にエラーになる。
        </li>
        <li>
          ビルド時に生成されたファイルを削除してくれる。
        </li>
      </ul>
    </li>
    
    <li>
      Titanium Studioのメニューより、Run > Run を実行して、シミュレータを起動する。
    </li>
    <li>
      AdMobが表示されればOK。
    </li>
    <li>
      exampleからコピーしてきた app.js を削除し、リネームしたものを app.js に戻す。
    </li>
  </ol>
  
  <h5>
    5. オリジナル app.js に追記する。
  </h5>
  
  <p>
    起動時に app.js がビューする場合は、以下を app.js に追記すれば良い。adviewをWindowに入れれば表示される。
  </p>
  
  <p>
    publisherID の部分は事前に <a href="http://jp.admob.com/" target="_blank">AdMobサイト</a>でアプリを登録した際に発行されるIDを入力してください。
  </p>
  
  <pre class="syntax-highlight">

<span class="synIdentifier">var</span> Admob = require(<span class="synConstant">'ti.admob'</span>);

<span class="synIdentifier">var</span> win = Ti.UI.createWindow(<span class="synIdentifier">{</span>

backgroundColor: <span class="synConstant">'white'</span>

<span class="synIdentifier">}</span>);



<span class="synIdentifier">var</span> adview = Admob.createView(<span class="synIdentifier">{</span>

<span class="synStatement">top</span>: 0, left: 0,

width: 320, height: 50,

adBackgroundColor: <span class="synConstant">'black'</span>,

testing: <span class="synConstant">true</span>,

keywords: <span class="synConstant">''</span>,

publisherId:<span class="synConstant">'***************'</span> <span class="synComment">// 取得したIDに置き換えてください。</span>

<span class="synIdentifier">}</span>);



win.add(adview);

</pre>
  
  <p>
    自分の場合、app.js は tabGroup の設定のみなので追記した先は app.js ではなく、tab 内の window で指定されているファイルにそれぞれ以下のように追記した。
  </p>
  
  <p>
    AdMob が上部で、その下が TableView を表示したかったので top を定義している。
  </p>
  
  <pre class="syntax-highlight">

<span class="synIdentifier">var</span> win = Ti.UI.currentWindow;



<span class="synComment">// Admob</span>

<span class="synIdentifier">var</span> Admob = require(<span class="synConstant">'ti.admob'</span>);

<span class="synIdentifier">var</span> adview = Admob.createView(<span class="synIdentifier">{</span>

<span class="synStatement">top</span>: 0, left: 0,

width: 320, height: 50,

adBackgroundColor: <span class="synConstant">'black'</span>,

testing: <span class="synConstant">true</span>,

keywords: <span class="synConstant">''</span>,

publisherId:<span class="synConstant">'a14f1d41c54f42c'</span> <span class="synComment">// Replace this string with your own API key!</span>

<span class="synIdentifier">}</span>);



win.add(adview);



<span class="synIdentifier">var</span> data = <span class="synIdentifier">[]</span>;

<span class="synIdentifier">var</span> tableView = Ti.UI.createTableView(<span class="synIdentifier">{</span>

data:data,

<span class="synStatement">top</span>:50, left:0,

width:320,

minRowHeight:58

<span class="synIdentifier">}</span>);



win.add(tableView);

</pre>
  
  <h5>
    6. シミュレータを起動してみる。
  </h5>
  
  <ol>
    <li>
      Titanium Studioのメニューより、該当のプロジェクトに対して、Project > Clean を実施する。
    </li>
    <li>
      Titanium Studioのメニューより、Run > Run を実行して、シミュレータを起動する。
    </li>
    <li>
      AdMobが表示されればOK。 <ul>
        <li>
          もしこれでもエラーが起きる場合は、Titanium Studioで新規プロジェクトを作成し、Resouces フォルダをコピーしてやり直すのが良いと思う。
        </li>
      </ul>
    </li>
  </ol>
  
  <p>
    以下のような感じで無事表示できましたと。
  </p>
  
  <p>
    <a href="http://f.hatena.ne.jp/d_sea/20120124122013" class="hatena-fotolife" target="_blank"><img src="http://cdn-ak.f.st-hatena.com/images/fotolife/d/d_sea/20120124/20120124122013.png?w=830" alt="f:id:d_sea:20120124122013p:image" title="f:id:d_sea:20120124122013p:image" class="hatena-fotolife" data-recalc-dims="1" /></a><a href="http://f.hatena.ne.jp/d_sea/20120124122012" class="hatena-fotolife" target="_blank"><img src="http://cdn-ak.f.st-hatena.com/images/fotolife/d/d_sea/20120124/20120124122012.png?w=830" alt="f:id:d_sea:20120124122012p:image" title="f:id:d_sea:20120124122012p:image" class="hatena-fotolife" data-recalc-dims="1" /></a>
  </p>
  
  <p>
    <h5>
      参考URL
    </h5>
    
    <ul>
      <li>
        大体の手順のイメージを捉えるのに役に立った</p> <ul>
          <li>
            <a href="http://m-50.jugem.jp/?eid=9" target="_blank"><a href="http://m-50.jugem.jp/?eid=9" target="_blank">http://m-50.jugem.jp/?eid=9</a></a>
          </li>
          <li>
            <a href="http://d.hatena.ne.jp/kaz_konno/20110305/1299344159" target="_blank"><a href="http://d.hatena.ne.jp/kaz_konno/20110305/1299344159" target="_blank">http://d.hatena.ne.jp/kaz_konno/20110305/1299344159</a></a>
          </li>
        </ul>
      </li>
      
      <li>
        Appcelerator Developer Blog <ul>
          <li>
            <a href="http://developer.appcelerator.com/blog/2011/07/building-apps-with-modules.html" target="_blank"><a href="http://developer.appcelerator.com/blog/2011/07/building-apps-with-modules.html" target="_blank">http://developer.appcelerator.com/blog/2011/07/building-apps-with-modules.html</a></a>
          </li>
          <li>
            <a href="http://developer.appcelerator.com/blog/2011/09/admob-for-android-module-tutorial-and-code.html" target="_blank"><a href="http://developer.appcelerator.com/blog/2011/09/admob-for-android-module-tutorial-and-code.html" target="_blank">http://developer.appcelerator.com/blog/2011/09/admob-for-android-module-tutorial-and-code.html</a></a>
          </li>
        </ul>
      </li>
    </ul></div>