---
layout: post
title: "WordPressプロフェッショナル養成読本という本を書きました"
date: 2014-11-18 08:00:00 +0900
comments: true
categories: [Book, Business]
---

[![WordPress本](/images/20141118-wordpress-book.jpg)](http://www.amazon.co.jp/gp/product/4774167878/ref=as_li_ss_tl?ie=UTF8&camp=247&creative=7399&creativeASIN=4774167878&linkCode=as2&tag=dsea-22)

本を書きました。WordPress関連の本になります。
株式会社シェイクソウルを作ってから2冊目になります。
合計7名による共著で、各章ごとに1筆者担当でした。

最初は会社を作ってすぐの頃に [「よくわかるAmazon EC2/S3」](http://www.amazon.co.jp/gp/product/4774142840/ref=as_li_ss_il?ie=UTF8&camp=247&creative=7399&creativeASIN=4774142840&linkCode=as2&tag=dsea-22) を共著したので、本を出すのは4年ぶりになります。

タイトルは、
[「WordPress プロフェッショナル 養成読本 [Webサイト運用の現場で役立つ知識が満載！] (Software Design plus)」](http://www.amazon.co.jp/gp/product/4774167878/ref=as_li_ss_tl?ie=UTF8&camp=247&creative=7399&creativeASIN=4774167878&linkCode=as2&tag=dsea-22)

エンジニアの方にはお馴染みの技術評論社から出ている Software Design 系列の雑誌形態で、"WordPressを幅広くより実践的に学ぶための本" というコンセプトです。

すでに発売中で、電子書籍化もされています。

[![WordPress プロフェッショナル 養成読本](http://ecx.images-amazon.com/images/I/610lYz15VZL.jpg)](http://www.amazon.co.jp/gp/product/B00OJVW65I/ref=as_li_ss_tl?ie=UTF8&camp=247&creative=7399&creativeASIN=B00OJVW65I&linkCode=as2&tag=dsea-22)

* [Amazon Kindle](http://www.amazon.co.jp/gp/product/B00OJVW65I/ref=as_li_ss_tl?ie=UTF8&camp=247&creative=7399&creativeASIN=B00OJVW65I&linkCode=as2&tag=dsea-22)
* [楽天 Kobo](http://hb.afl.rakuten.co.jp/hgc/1360a562.9a5abdd9.1360a563.41a92e76/?pc=http%3a%2f%2fitem.rakuten.co.jp%2frakutenkobo-ebooks%2f9a39447a0f8b32cfa7753d907d110ab3%2f%3fscid%3daf_link_txt&amp;m=http%3a%2f%2fm.rakuten.co.jp%2frakutenkobo-ebooks%2fi%2f13693951%2f)

<!-- more -->

#  馴れ初め

2013年末あたりに技術評論社の方からメールで連絡がありました。
どうやら [スマートWP][] を見ていただいて、サービスの特徴である "AWSでスケーラブルWordPress" について書いて欲しいということでした。

確かにWordPressをAWS上で積極的に動かしているのはAmimotoとスマートWPくらいですし、AWSオートスケーリング自体の紹介も少ない中、WordPress に適用させようとするマニアックなことを紹介しているのは日本でもなかなかないのかなと思います。

WordPress関連で出版されている本は、WordPress入門とデザインに関するものが多く、技術的なノウハウや運用上必要な要素については少ないと思っていました。今回の本はすでに出版されている本では触れられていない、**"WordPressを実践的に使うための本"** にしたいということでしたので、興味を持ちました。

自分自身のノウハウがどの程度外部の方に役立つのか試してみたかったという思いと、アウトプットすることで自分自身の整理にもなり理解が一段と深くなるのではないかと思い、引き受けることにしました。

# 書いた内容について

書いた章のタイトルは **"AWSでスケーラブルWordPress"** です。

AWSを用いてWordPressを動かす、しかもオートスケーリングさせて人の手を介在させないスケーリングの自動化を実現します。

誰もがこの構成を目指すものではなく、はっきり言ってマニアックですｗ

網羅している技術範囲も広く、スケーラブルなサーバ設計、AWS(EC2, S3, ELB, オートスケーリング, RDS)、WordPress、必要なツール(S3fs, Dropbox) が登場します。

なるべく構築手順を示して、本を手にした方が実際その通りの手順をするとスケーラブルなWordPressサイトが構築できるように心がけて書きました。
はじめて書いた本でもオートスケーリングについて書きましたが、今回はWebUIから操作できるようになっていたり最新の設定方法を反映できています。

ここで構築している構成は実際、[スマートWP][] で利用していますが、Dropbox でファイルをデプロイするユニークな方法をとっています。
これによって手元のPCから即時に複数のサーバでも反映できるリアルタイムデプロイが可能になります。
Dropboxリアルタイムデプロイは [Smart Designing][] で静的HTMLファイルのWebホスティング環境として提供しています。

# オートスケーリングコンサルティング開始！

[![オートスケーリングコンサルティング](http://www.shakesoul.net/wp-content/uploads/2013/10/ss-autoscaling.png)](http://www.shakesoul.net/portfolio/consulting/autoscaling-consulting)

オートスケーリング技術は、サーバ設計ノウハウがベースになりつつ、AWSの各サービスの把握も必要で学習コストが高い技術です。
日本ではオートスケーリングという魔法のような技術を積極的に導入しようとする動きが少ないようですので、興味はあるけどよくわからない状況かと思っています。

今回の執筆でオートスケーリングのノウハウがまた一段とたまりましたし、ノウハウをしっかり自分の会社でも活かしていこうということで、**[オートスケーリングコンサルティング][]** をサービスとして始めています。

ご興味あればコンタクトページから連絡いただければです。

# サイン会したよ

[WordCamp 2014 Tokyo](http://2014.tokyo.wordcamp.org/) の会場でサイン会をしました。

![WordCamp 2014 Tokyo サイン会](/images/20141118-wordpress-book-sign.jpg)

共著した他の筆者の方々に会うのははじめてだったのですが、顔見知りもいて何やら懐かしいやらお疲れ様な感じやらでした。

# 謝辞

この本は雑誌の形式をとっているため、謝辞と著者プロフィールがありませんでしたので、ここで謝辞を述べたいと思います。

今回の執筆にあたり、原稿のレビューを外部の方にお願いして客観評価を加えながらブラッシュアップしていきました。
外国の著名なビジネス書は必ず出版前の原稿を外部の友人・知人、そのジャンルの知識人へレビューを依頼し、文章をより良いものにしています。今回の執筆は、それに習って出版前のレビューをWordPressコミュニティに精通されている方にお願いしました。

この場を借りてレビューをしてくださった2名にお礼を言いたいと思います。

[H2O Spaceのたにぐちまことさん](https://h2o-space.com/) には、豊富な執筆経験より文法的なチェックや分かりやすい言い回しを教えていただきました。

[ツクリべの小川さん](http://www.tsukuri.be/) は、実際原稿に示した作業手順通りにわざわざ操作してみていただいて、手順や解説のわかりやすさをユーザ視点でレビューいただきました。

このお二人の質の高く、僕の身に余る献身的なレビューのお陰で文章の質が1つも2つも上がったものになりました。本当にありがとうございました！！

個人的にはレビューによって読み手側の感想をダイレクトに聞くことができたので、自分のアウトプットのわかりやすさのチェックや言い回しのクセみたいなものを知ることができました。

事前にレビューを依頼する試みははじめての事だったのですが、やはり **"客観評価がアウトプットをより良くしてくれる"** という良い体験ができました。

# 他の章について

仕上がった本を読んでみると、WordPressの今からSEO、PHPプログラミングまでWordPressを中心として派生する各話題を幅広く網羅していると思います。

WordPressはアップデートが早いのでなかなか最新を追うのは苦労しますが、**WordPressの今** が知れるのは結構貴重かと思います。REST APIなどはまだ出てきたばかりの機能ですし、今後どのようにWordPressが変化していくのかを知れると思います。

個人的にはNginx入門とセキュリティは勉強になりました。特にNginxは使っていますが、だいたい検索したり[Niginx.orgのWiki](http://wiki.nginx.org/Main)を見ながら我流で設定していたので、知らない記述の仕方に触れることができました。

# まとめ

1つ1つのテーマはそれぞれ単独の本が書けるくらい重要なので、紙面の限られる雑誌形態の1冊でまとめるのは難しいかもしれません。本を手にした方にとっては全てのテーマが必要ないかもしれません。

ただ、WordPressを実践的扱っていく場合にはどのテーマも外せないものですので、今まで初期インストールで満足してしまっていた方々には、実践するための知識やノウハウを全般的に知れる本になるかと思います。

私の書いた AWSでスケーラブルWordPress は同じような内容で述べられているものはそうそうないと思いますし、概要説明から実際手を動かして構築できる手順までなるべく分かりやすく示したつもりです。

興味があればその部分に触れるためにだけに買っていもらえれば嬉しいです。

フィードバックはこのブログのコメントでもいただければです。




 [スマートWP]: http://smtwp.com/
 [Smart Designing]: http://smartdesigning.me/
 [オートスケーリングコンサルティング]: http://www.shakesoul.net/portfolio/consulting/autoscaling-consulting
