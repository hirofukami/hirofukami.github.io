---
title: trac plugin 追加 (timing and estimation, ScrumBurndown)
date: Tue, 12 Feb 2008 15:00:00 +0000
author: Hiro Fukami
layout: post
permalink: /2008/02/12/trac-plugin-timing-and-estimation-scrumburndown/
tumblr_hirofukami_permalink:
  - http://hirofukami.tumblr.com/post/29609165548/trac-plugin-timing-and-estimation-scrumburndown
  - http://hirofukami.tumblr.com/post/29609165548/trac-plugin-timing-and-estimation-scrumburndown
  - http://hirofukami.tumblr.com/post/29609165548/trac-plugin-timing-and-estimation-scrumburndown
  - http://hirofukami.tumblr.com/post/29609165548/trac-plugin-timing-and-estimation-scrumburndown
tumblr_hirofukami_id:
  - 29609165548
  - 29609165548
  - 29609165548
  - 29609165548
original_post_id:
  - 403
  - 403
  - 403
  - 403
categories:
  - Tech
tags:
  - Server
---
<div class="section">
  <p>
    <a href="http://d.hatena.ne.jp/d_sea/20070720" target="_blank">以前にも</a> trac に入れておいたほうがいい plugin をのせたが、チケットに対して目標日時だけでなく、もっとシビアに時間管理、工数管理がしたい場合に入れておいたほうがいい plugin を教えてもらったので入れてみた。今回もインストールに難儀してしまったなぁ。。。
  </p>
  
  <p>
    ということで、細かめに手順をメモ。細かなミスはご愛嬌。。。
  </p>
  
  <ul>
    <li>
      前提として</p> <ul>
        <li>
          trac のプロジェクト作成済み
        </li>
        <li>
          Webadmin インストール済み
        </li>
        <li>
          /hogehoge/project/conf/trac.ini と /hogehoge/project/db/trac.db へ apache からの書き込み権限があること <ul>
            <li>
              /hogehoge/project は適当に読み変えて下さい
            </li>
          </ul>
        </li>
        
        <li>
          trac の log が見れること <ul>
            <li>
              Webadmin で Logging&#160;: [Type: File], [Log level: DEBUG] にしておくといいかも
            </li>
          </ul>
        </li>
      </ul>
    </li>
  </ul>
  
  <h4>
    timing and estimation インストール
  </h4>
  
  <p>
    最初に timing and estimation をインストールする。
  </p>
  
  <ul>
    <li>
      <a href="http://trac-hacks.org/wiki/TimingAndEstimationPlugin" target="_blank">ここ</a> の Getting the Plugin &#8212; Download the zipped source&#160;: から該当の zip ファイルをダウンロード(動かしているTracのバージョンが分かっていること)
    </li>
    <li>
      適当なディレクトリで以下を実行
    </li>
  </ul>
  
  <blockquote>
    <p>
      # unzip timingandestimationplugin-branches-trac0.10.zip
    </p>
    
    <p>
      # cd timingandestimationplugin/branches/trac0.10/
    </p>
    
    <p>
      # python setup.py bdist_egg
    </p>
  </blockquote>
  
  <ul>
    <li>
      生成された egg ファイルをプロジェクトの plugin ディレクトリにコピー
    </li>
  </ul>
  
  <blockquote>
    <p>
      # cd ./dist/
    </p>
    
    <p>
      # cp timingandestimationplugin-0.5.3-py2.4.egg /hogehoge/project/plugins/
    </p>
  </blockquote>
  
  <ul>
    <li>
      chmod, chown する
    </li>
  </ul>
  
  <blockquote>
    <p>
      # cd /hogehoge/project/plugins/
    </p>
    
    <p>
      # chmod 755 timingandestimationplugin-0.5.3-py2.4.egg
    </p>
    
    <p>
      # chown apache:apache timingandestimationplugin-0.5.3-py2.4.egg
    </p>
  </blockquote>
  
  <ul>
    <li>
      httpd 再起動
    </li>
  </ul>
  
  <blockquote>
    <p>
      # /etc/rc.d/init.d/httpd graceful
    </p>
  </blockquote>
  
  <ul>
    <li>
      ブラウザでアクセス</p> <ul>
        <li>
          おそらく 500 が返ってきて、error log を見ろと言われる
        </li>
        <li>
          trac の log をチェック おそらく DabataseError が出ているので、trac-admin で upgrade かける
        </li>
      </ul>
    </li>
  </ul>
  
  <blockquote>
    <p>
      # trac-admin /hogehoge/project upgrade
    </p>
  </blockquote>
  
  <ul>
    <ul>
      <li>
        再度、ブラウザアクセス Admin &#8212; Plugins で追加されていることを確認
      </li>
      <li>
        timingandestimationplugin をクリックして、すべてチェックボックスを入れて &#8220;Apply changes&#8221;
      </li>
    </ul>
  </ul>
  
  <h4>
    ScrumBurndown インストール
  </h4>
  
  <ul>
    <li>
      <a href="http://trac-hacks.org/wiki/ScrumBurndownPlugin" target="_blank">ここ</a> の Download から zip ファイルをダウンロード(生 egg ファイルだと python のバージョンが合わないために動かない可能性あり)
    </li>
    <li>
      適当なディレクトリで以下を実行
    </li>
  </ul>
  
  <blockquote>
    <p>
      # unzip burndown_src.zip
    </p>
    
    <p>
      # cd burndown_src
    </p>
    
    <p>
      # python setup.py bdist_egg
    </p>
  </blockquote>
  
  <ul>
    <li>
      生成された egg ファイルをプロジェクトの plugin ディレクトリにコピー
    </li>
  </ul>
  
  <blockquote>
    <p>
      # cd ./dist/
    </p>
    
    <p>
      # cp TracBurndown-01.06.10-py2.4.egg /hogehoge/project/plugins/
    </p>
  </blockquote>
  
  <ul>
    <li>
      chmod, chown する
    </li>
  </ul>
  
  <blockquote>
    <p>
      # cd /hogehoge/project/plugins/
    </p>
    
    <p>
      # chmod 755 TracBurndown-01.06.10-py2.4.egg
    </p>
    
    <p>
      # chown apache:apache TracBurndown-01.06.10-py2.4.egg
    </p>
  </blockquote>
  
  <ul>
    <li>
      httpd 再起動
    </li>
  </ul>
  
  <blockquote>
    <p>
      # /etc/rc.d/init.d/httpd graceful
    </p>
  </blockquote>
  
  <ul>
    <li>
      ブラウザでアクセス</p> <ul>
        <li>
          おそらく 500 が返ってきて、error log を見ろと言われる
        </li>
        <li>
          trac の log をチェック おそらく DabataseError が出ているので、trac-admin で upgrade かける
        </li>
      </ul>
    </li>
  </ul>
  
  <blockquote>
    <p>
      # trac-admin /hogehoge/project upgrade
    </p>
  </blockquote>
  
  <ul>
    <ul>
      <li>
        再度、ブラウザアクセス Admin &#8212; Plugins で追加されていることを確認(すでに Enabled になっているはず)
      </li>
    </ul>
    
    <li>
      ユーザ権限追加 <ul>
        <li>
          Admin &#8212; permissions で Burndown への view と admin 権限を追加</p> <ul>
            <li>
              Grant Permission: Subject: (ユーザ名)
            </li>
            <li>
              Action: BURNDOWN_VIEW or BURNDOWN_ADMIN を選んで、&#8221;Add&#8221; を押す
            </li>
          </ul>
        </li>
      </ul>
    </li>
    
    <li>
      権限のあるユーザでログインして、ブラウザ上で上部メニュー内に &#8220;Management&#8221; が見えればOK
    </li>
  </ul>
  
  <h4>
    参考URL
  </h4>
  
  <ul>
    <li>
      pluginのソースとインストール方法</p> <ul>
        <li>
          <a href="http://trac-hacks.org/wiki/TimingAndEstimationPlugin" target="_blank"><a href="http://trac-hacks.org/wiki/TimingAndEstimationPlugin" target="_blank">http://trac-hacks.org/wiki/TimingAndEstimationPlugin</a></a>
        </li>
        <li>
          <a href="http://trac-hacks.org/wiki/ScrumBurndownPlugin" target="_blank"><a href="http://trac-hacks.org/wiki/ScrumBurndownPlugin" target="_blank">http://trac-hacks.org/wiki/ScrumBurndownPlugin</a></a>
        </li>
        <li>
          <a href="http://simplate.aimy.jp/wiki/TracPlugins" target="_blank"><a href="http://simplate.aimy.jp/wiki/TracPlugins" target="_blank">http://simplate.aimy.jp/wiki/TracPlugins</a></a> [08/02/14追記]
        </li>
      </ul>
    </li>
    
    <li>
      インストール方法 <ul>
        <li>
          <a href="http://d.hatena.ne.jp/digo/20071027/1193473053" target="_blank"><a href="http://d.hatena.ne.jp/digo/20071027/1193473053" target="_blank">http://d.hatena.ne.jp/digo/20071027/1193473053</a></a>
        </li>
        <li>
          <a href="http://ameblo.jp/itboy/entry-10039245666.html" target="_blank"><a href="http://ameblo.jp/itboy/entry-10039245666.html" target="_blank">http://ameblo.jp/itboy/entry-10039245666.html</a></a>
        </li>
      </ul>
    </li>
  </ul>
  
  <h5>
    08/02/14修正
  </h5>
  
  <p>
    # python setup.py install => # python setup.py bdist_egg へ修正
  </p>
</div>