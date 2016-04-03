---
layout: post
title: "MacBook 12インチ 電源ON時の音を消す方法"
date: 2015-11-25 12:00:00 +0900
comments: true
categories:
  - Apple
tags:
  - MacBook
  - PowerChime
---

MacBook 12インチを使っていつくかの不満点があるけど、その一つが電源アダプターをコンセントに挿して通電すると「ポンッ♪」という音が鳴る。

![MacBook 12inch](/images/2015/11/20151125-macbook-12inch.JPG)

この音は MacBook の音量を消音にしていても鳴るし、閉じてスリープ状態でも鳴る。音量も調節できない。

MacBook 12インチは電源ポートが USB-C 1つだけで今まであった通電確認できる LED がなくなっている。
その代わりにユーザにに何かしら通知するために、この音を鳴らすようにしたというアップルの意図かと思われる。

だけど、制御できない音を勝手に鳴らされるのはよろしくない。私語厳禁のような静かなワークスペースでこの音が鳴ってしまうので非常に困っていた。

いい加減不満が溜まってきたので対応して、この音が鳴らないようにできたのでメモを残しておく。

# PowerChime

![Activity Monitor before](/images/2015/11/20151125-powerchime-before.png)

この音を鳴らしているのは PowerChime.app というアプリケーション。
アクティビティーモニターで見ると、ユーザ権限で起動していることがわかる。

このままプロセスを落とせば、チャイムはならなく鳴る。しかし、OS起動時に PowerChime.app が立ち上がるようになっているので、OS再起動しても鳴らないようにしたい。

<!-- more -->

# 作業前

ここからはターミナルで作業する。

## 確認

```bash
$ ps aux | grep -i powerchime | grep -v grep
hironobu          913   0.0  0.2  2533460  15704   ??  S    10:16AM   0:00.12 /System/Library/CoreServices/PowerChime.app/Contents/MacOS/PowerChime
```

確かにプロセスが起動している。

## plist

各プロセス起動の管理は plist ファイルにてXML形式で記述されている。

root が起動するプロセスは `/System/Library/LaunchDaemons/` 配下、<br>
ユーザが起動するプロセスは `/System/Library/LaunchAgents/` 配下、<br>に plist ファイルが置かれている。

今回 PowerChime はユーザ権限でプロセス起動していたので、
plist の場所は、<br> `/System/Library/LaunchAgents/com.apple.powerchime.plist` にある。

中身を見てみる。

```bash
$ more /System/Library/LaunchAgents/com.apple.powerchime.plist
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
        <key>LimitLoadToSessionType</key>
        <array>
                <string>LoginWindow</string>
                <string>Aqua</string>
        </array>
        <key>RunAtLoad</key>
        <true/>
        <key>Label</key>
        <string>com.apple.powerchime</string>
        <key>Program</key>
        <string>/System/Library/CoreServices/PowerChime.app/Contents/MacOS/PowerChime</string>
        <key>ProcessType</key>
        <string>Interactive</string>
</dict>
</plist>
```

こんな感じの記述。

## launchctl コマンド

OS起動時のプロセス管理のために `launchctl` コマンドを使う。

`launchctl` は OS起動時のプロセスを管理するためのコマンド。
`launchctl (Enter)` でコマンドの説明が確認できる。

現在、OS起動時に立ち上がるプロセス一覧を確認したい場合は `launchctl list` コマンドを使う。

```bash
$ launchctl list | grep -i powerchime
894     0       com.apple.powerchime
```

確かに PowerChime がOS起動時に立ち上がるように設定されている。


OS起動時にこの plist を読まないようにするには `launchctl unload` に `-w` オプションを付ける。
OS起動時に立ち上がらないと同時に現在起動している場合はそのプロセスも落とす。

以下、コマンドのヘルプ。

```bash
$ launchctl unload -h
Usage: launchctl unload <service-path, service-path2, ...>
        -w      Additionally disables the service such that future load
                operations will result in a service which launchd tracks but
                cannot be launched or discovered in any way.
        -S <session>
                Only unloads the services associated with the specified session.
        -D <domain>
                Unloads launchd.plist(5) files from the specified domain. See
                the discussion regarding this same flag when given to the load
                subcommand for further details.
```
先ほど見た plist ファイルのパスを指定する。

実行してみる。ユーザプロセスなので `sudo` なしでOK。

```bash
$ launchctl unload -w /System/Library/LaunchAgents/com.apple.powerchime.plist
```


# 実行後

## 確認

![Activity Monitor after](/images/2015/11/20151125-powerchime-after.png)

```bash
$ ps aux | grep -i powerchime | grep -v grep
```

PowerChime プロセスが落ちている。

一応 plist ファイルに変更が加えられたか見てみる。

```bash
$ more /System/Library/LaunchAgents/com.apple.powerchime.plist
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
        <key>LimitLoadToSessionType</key>
        <array>
                <string>LoginWindow</string>
                <string>Aqua</string>
        </array>
        <key>RunAtLoad</key>
        <true/>
        <key>Label</key>
        <string>com.apple.powerchime</string>
        <key>Program</key>
        <string>/System/Library/CoreServices/PowerChime.app/Contents/MacOS/PowerChime</string>
        <key>ProcessType</key>
        <string>Interactive</string>
</dict>
</plist>
```

特に変わっていないみたい。

```bash
$ launchctl list | grep -i powerchime
```

OS起動時のプロセスから PowerChime が外れた。


# 起動させたい時

もし、OS起動時に立ち上がるようにしたい場合は(個人的にはないケースだけど)、 `launchctl load` コマンドに `-w` オプションを付ける。

```bash
$ launchctl load -w /System/Library/LaunchAgents/com.apple.powerchime.plist
```

OS起動時に立ち上がるようになると同時にプロセスも起動する。


# 参考情報

[http://www.maruko2.com/mw/LaunchDaemons_(launchctl,_launchd.plist)_の使い方](http://www.maruko2.com/mw/LaunchDaemons_\(launchctl,_launchd.plist\)_の使い方)