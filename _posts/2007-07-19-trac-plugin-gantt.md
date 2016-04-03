---
title: trac に入れときたい plugin (Gantt)
date: Thu, 19 Jul 2007 15:00:00 +0000
author: Hiro Fukami
layout: post
permalink: /2007/07/19/trac-plugin-gantt/
tumblr_hirofukami_permalink:
  - http://hirofukami.tumblr.com/post/29609154462/trac-plugin-gantt
  - http://hirofukami.tumblr.com/post/29609154462/trac-plugin-gantt
  - http://hirofukami.tumblr.com/post/29609154462/trac-plugin-gantt
  - http://hirofukami.tumblr.com/post/29609154462/trac-plugin-gantt
tumblr_hirofukami_id:
  - 29609154462
  - 29609154462
  - 29609154462
  - 29609154462
original_post_id:
  - 432
  - 432
  - 432
  - 432
categories:
  - Tech
tags:
  - Server
---
<div class="section">
  <p>
    trac をよく知らないまま3月くらいから使っているわけだけど、Webadmin以外に入れておいたほうがいいプラグインがあった。
  </p>
  
  <p>
    trac はデフォルトだとチケットごとにリミットの日付情報が載せれないので、マイルストーンだけの日付管理はいまいちだなぁと思っていたらちゃんとあった。
  </p>
  
  <p>
    インストールと trac.ini の追記は <a href="http://wiki.livedoor.jp/syo1976/d/GanttPlugin" target="_blank"><a href="http://wiki.livedoor.jp/syo1976/d/GanttPlugin" target="_blank">http://wiki.livedoor.jp/syo1976/d/GanttPlugin</a></a> を参考に
  </p>
  
  <p>
    上記だとダウンロード先リンクが切れていたので、<a href="http://www.hsbt.org/diary/20060928.html" target="_blank"><a href="http://www.hsbt.org/diary/20060928.html" target="_blank">http://www.hsbt.org/diary/20060928.html</a></a> からたどって落とした。
  </p>
  
  <p>
    conf/trac.ini の設定は
  </p>
  
  <ul>
    <li>
      Webadmin を動かすには
    </li>
  </ul>
  
  <blockquote>
    <p>
      webadmin.* = enabled
    </p>
  </blockquote>
  
  <ul>
    <li>
      Gantt を動かすには
    </li>
  </ul>
  
  <blockquote>
    <p>
      tracgantt.* = enabled
    </p>
  </blockquote>
  
  <p>
    を記述
  </p>
  
  <p>
    Webadmin で変更した内容が反映されるために、書き込み権限を忘れなく。
  </p>
  
  <h5>
    (追記 07/08/09)実際使い始めてみるともっといろいろ必要になった
  </h5>
  
  <dl>
    <dt>
      IniAdmin
    </dt>
    
    <dd>
      conf/trac.ini をブラウザ上から変更できるようになる。ターミナル立ち上げてわざわざsshで入って、、見たいなことをせずに済むのですごい便利。<a href="http://trac-hacks.org/wiki/IniAdminPlugin" target="_blank"><a href="http://trac-hacks.org/wiki/IniAdminPlugin" target="_blank">http://trac-hacks.org/wiki/IniAdminPlugin</a></a> インストール方法は通常の plugin と同じ
    </dd>
    
    <dt>
      notification
    </dt>
    
    <dd>
      チケットの更新をメールで通知してくれる。RSSだけだと自分が担当しているか分からないし、更新の差分情報も分からないので、実質これがないと使えない。plugin ではなく、trac.ini の [notification] セクションで設定することで利用可能。SMTPサーバ、アカウント、パスワード情報が必要 # たぶんアカウント情報は使ってないと思うのだが、、
    </dd>
    
    <dt>
      担当者のプルダウン選択
    </dt>
    
    <dd>
      担当者欄に入力する際誰がいるのか分からないと入力のしようもないので、プルダウンで選べるようにできる。これも trac.ini の [ticket] セクションの restrict_owner オプションを &#8220;true&#8221; に設定すれば利用可能。ただし、ユーザ設定でアカウントとメールアドレスを保存したユーザのみがリストに入るので担当者になりうる人はユーザ設定してもらう必要あり。
    </dd>
  </dl>
  
  <p>
    何気にplugin以外は trac の ヘルプ/Guide に書いてあったんだよね。ちゃんと読めばよかったか、、、が必要に迫られないとちゃんと読まないので、これでよしか。
  </p>
</div>