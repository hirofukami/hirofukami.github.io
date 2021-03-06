---
title: ロリポップ WordPress 大規模改ざんの原因予想と思ふところ
date: Mon, 02 Sep 2013 14:49:48 +0000
author: Hiro Fukami
layout: post
permalink: /2013/09/02/lolipop-wordpress-trouble/
tagazine-media:
  - 'a:7:{s:7:"primary";s:0:"";s:6:"images";a:0:{}s:6:"videos";a:0:{}s:11:"image_count";i:0;s:6:"author";s:7:"8120754";s:7:"blog_id";s:8:"48436223";s:9:"mod_stamp";s:19:"2013-09-02 08:41:18";}'
  - 'a:7:{s:7:"primary";s:0:"";s:6:"images";a:0:{}s:6:"videos";a:0:{}s:11:"image_count";i:0;s:6:"author";s:7:"8120754";s:7:"blog_id";s:8:"48436223";s:9:"mod_stamp";s:19:"2013-09-02 08:41:18";}'
categories:
  - Business
tags:
  - lolipop
  - security
  - thinking
  - trouble
  - wordpress
---
<a href="http://www.itmedia.co.jp/news/articles/1308/29/news093.html" target="_blank">「ロリポップ」でWordPress利用サイトが改ざん　「大規模攻撃」で被害4802件</a>

現時点では、まだ完全に収束していなくて、ロリポップはこの件の<a href="http://lolipop.jp/info/news/4149/" target="_blank">アップデートをお知らせし続けている</a>。

この原因は「WordPressのテーマかプラグインの脆弱性からファイルパーミッションの設定不備をつかれた」とのことだけど、これだけでは不明確で、追々報告はあるかもしれないが、いまだ全容は明らかになっていない。  
<a href="http://www.itmedia.co.jp/news/articles/1308/30/news127.html" target="_blank">「ロリポップ」のWordPressサイト改ざん被害、原因はパーミッション設定不備</a>

という状況なのだけど、WordPress 制作者の皆さんは何が起きてどうすればいいのか、イマイチ理解されていない気がして、リアクションできずに固まって、エンドユーザへの説明もなかなかつらいところなのではと思う。

もう少し現象が分かるように、予想もあるけども追ってみたいと思う。今後共有レンタルサーバを提案して良いものなのかどうか、みなさんのお仕事の参考なれば良いと思う。

## 予想される流れ

詳細が公表されていないので、具体的にはなんとも言えないけど、ロリポップの公表情報から予測してみると以下の流れがあるのではと思う。

1.  あるユーザが自分の WordPress サイトに悪意のある非公式なテーマ or プラグインをサーバにおいた
2.  そのテーマ or プラグインがサーバ内で動作する
3.  そのサーバに共有されている他ユーザのディレクトリ名が分かる
4.  Apache の mod_rewrite 上のオプション FollowSymLinks が有効になっていたので、他ユーザの wp-config.php へのパスを予測してシンボリックリンクを貼って http 経由でアクセスして wp-config.php の内容を見る
5.  サーバ内 or 外部から DB サーバへアクセスし、テーブルの内容を書き換える
6.  4,5 の繰り返し

## 現象の原因要素

この流れが成立するためにはいくつかの要素必要。

1.  ロリポップのパーミッション設定不備
2.  共有サーバという形態
3.  WordPress の決められたファイルパスと情報

この3つがそれぞれ成立したことで現象は生じた。

## ロリポップのパーミション設定不備

対応内容で FollowSymLinks から SymLinksIfOwnerMatch に変えたとあったので、これで他ユーザの wp-config.php の内容を盗めたのではと思う。お互いのユーザのディレクトリ内は見えないようになっている状態だったが、そこをシンボリックリンクで http で飛び越えてしまう。  
詳細はわからないが、サーバ内部ではシンボリックリンクを */[他ユーザディレクトリ名]/wp-config.php* あてに貼りまくって、http アクセスして中身を見るということかな？と思っている。

技術的に実証して くださっている方がいた : <a href="http://blog.tokumaru.org/2013/09/symlink-attack.html" target="_blank">ロリポップのサイト改ざん事件に学ぶシンボリックリンク攻撃の脅威と対策</a>  
そもそもこの設定不備がなければ事象は起きなかったと思う。

## 共有サーバという形態

共有サーバは ssh login してみるとわかるけど、同居している他ユーザのディレクトリ名が分かってしまう。  
ssh login して *$ ls -l ../* すれば見えるはず。これは共有サーバであればどこもできるはず。致し方ない部分。

サーバは共有なのだけど Web サーバは各ユーザそれぞれ独立して動かすことができるように Web サーバには suEXEC が設定されている。  
また、他ユーザのディレクトリ配下のファイルが見えたり Web アプリからアクセスできてしまうとまずいので、全ユーザを1つのグループに入れてグループパーミッションを与えないようにすれば成立できる。だから今回で言うとパーミッションは 404 とか 400 にしましょうと2桁目が0にするようにサーバ運用会社は推奨している。これはセキュアにするための運用上の理由。

おそらく今回改ざんされちゃった人の wp-config.php のファイルパーミッションはグループ(2桁目)が Read(4) 以上のアクセスを付与してあったのだろう。それが結構な人数いたからこれだけの被害の大きさになったと思う。  
結構パーミッションは見落としがちで、自分のローカルPC上で wp-config.php を編集して DB 情報を記入して保存。この時点でパーミションは 644 とか になってしまって、グループに Read が与えられてしまい、そのまま気づくことなく FTP でサーバにアップロードしてしまう。

サーバ内においてはユーザのディレクトリのパーミッションはグループは 0 に設定されているので、たとえその下部にある wp-config.php のグループパーミッションに Read が付いていたとしても覗けないのだが、おそらく FollowSymLinks を使って、シンボリックリンクごしに http 経由でファイルにアクセスできたのではと思う。

## WordPress の決められたファイルパスと情報

WordPress は接続する DB の情報を wp-config.php に書く。そしてこの wp-config.php ファイルは Web で公開するディレクトリの最上部の階層に設置するケースがほどんどだろう。  
wp-config.php に DB の情報を書くことは WordPress 上の決まりなので、しょうがないところではある。  
そして、この決まりは知れ渡っているので狙われやすい状況ではある。

DB の情報を抜き出せればあとは、その情報を使って DB にログインして DB 内のデータを update する。ちょっと気になるのがこれは共有サーバからアクセスしたのではなくインターネット外部からなのでは？というところ。今回サーバ上のファイルはそのままだったようなのでサーバにログインしないで実行している気がする。DB に対するファイアウォールが内部の Web サーバからだけに絞られていないで、インターネット上にさらした状態であったとすれば、これもサーバ運用会社の不備ということになる。

## まとめると

サマリー的に言うと、

*   同一サーバ上の他ユーザのディレクトリ名一覧が入手できるという、共有サーバのセキュリティ上のリスクと
*   WordPress の wp-config.php という特定のファイル名と重要な情報を持っていることが知れ渡っていること。
*   というセキュリティリスクの高い状況下で、
*   FollowSymLinks の設定不備が引き金になって起こった。

というところだろう。

## そして、もう時代はクラウドへの移行なのではないか？

共有サーバのサービスはもう10年以上提供されていて、使われている技術は変わらず当時の古い、いわゆる枯れた技術で運用され続けている。クラウド登場前、Web1.0時代のホスティング手段で、物理サーバしかない時に安くホスティングサービスを提供するかという視点で作られている。

セキュリティを考えるなら1サーバに複数ユーザではなく、1サーバ1ユーザの形態で他ユーザの影響が自分に及ばない環境を求めるべきで、今なら低いスペックのVPSや1OS上でコンテナでより独立した環境を提供できる。より新しい技術やツールは出てきているのが現状。

いい加減古い技術から離れたほうがいいのでは、というのが私の意見。古いものは必ずセキュリティリスクが高くなっていく。おそらく今回の事象で FollowSymLinks は無効化しても、.htaccess 内に書いたら？とかそれをさせないためにサーバ運用会社はガイドラインを示すだろうけど、他社も含めリスクの指摘はあったが、今頃あたふたというのはちょっとまずいだろう。個人利用ならまだしも、ビジネスで使うレベルではないと思っている。

今回の事象は古いサービスを使い続けることからクラウドへ移行することを促す一つの警鐘と捉えてもいいと思う。

おそらく共有サーバを今でも選定される理由は、安さ、サーバ知識のいらない環境(ワンクリックインストールやWeb UIからの操作など)からだ思うが、クラウドの専用サーバ環境で同じような使い勝手を PaaS/SaaS として提供することは十分可能。日本では VPS の宣伝が大きくて、その上のレイヤまでカバーしてサーバレイヤを抽象化するようなサービスが少ないので、共有サーバに流れてしまっていると思う。

私がカバーしたいと思っているのはまさにこのサーバを意識しない抽象化されたクラウドサービスになる。しかも、これを簡単に提供すること。今クローズドβリリース準備中の <a href="http://www.shakesoul.net/smartwordpress" target="_blank">スマートWordPress</a> は、そんなサーバを意識しない抽象化した WordPress レイヤを簡単に提供するクラウドサービスです。  
クラウドのメリットを十分に活かして「必要な分を、必要な時に」動かして安心してサイト運営できるようにしている。確かに、共有サーバの500円〜2000円のレンジに比べれば高額になる予定だが、より本格的なサイト運営を求める方々の良きツールになりたいと思っている。

興味のある方は <a href="http://www.shakesoul.net/smartwordpress" target="_blank">スマートWordPress</a> のページでメール登録をお願いします。クローズドβリリースのご案内をさせていただきます。