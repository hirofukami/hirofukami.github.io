---
title: Chef solo knife コマンド覚書
date: Fri, 31 Jan 2014 10:53:16 +0000
author: Hiro Fukami
layout: post
permalink: /2014/01/31/chef-solo-knife-command-memo/
categories:
  - Tech
tags:
  - chef
  - command
  - Infra
  - memo
  - Server
  - tech
---
<img src="http://upload.wikimedia.org/wikipedia/en/5/56/Chef_Software_Inc._company_logo.png" width="360">

どうも Chef コマンドを覚えられず、入門 Chef solo を毎回読みなおしてしまっているので、コマンド覚書を作っておく。

*   レポジトリ名 : chef-repo
*   chef クライアントホスト名 : chef-client
*   クックブック名 : cookbook-name

適当に置き換えてください。

# 新規

## レポジトリ作成

Chef レポジトリを作る。

<pre class="brush: plain; title: ; notranslate" title="">$ knife solo init [chef-repo-name]</pre>

## クックブック作成

レシピを新規作成する。

<pre class="brush: plain; title: ; notranslate" title="">$ knife cookbook create [cookbook-name] -o site-cookbooks</pre>

## ノード設定

chef クライアントに仕立てる。

<pre class="brush: plain; title: ; notranslate" title="">$ knife solo prepare [chef-client-hostname]</pre>

AWS EC2 に適用する場合はキーとユーザ名の指定が必要なので、ec2-user ユーザで作業すると、

<pre class="brush: plain; title: ; notranslate" title="">$ knife solo prepare -i ~/Keypair/[key-file] ec2-user@[chef-client-hostname]</pre>

とする。

nodesディレクトリに [chef-client-hostname].json が作成される。

# 更新

## レシピを編集する

site-cookbooks/[cookbook-name]/recipes/default.rb を編集する。 適用したいレシピが複数に渡る場合はそれぞれの recipes/default.rb を編集する。

## 適用したいnodeファイルを確認

AWS EC2 AMI からの起動する際のように、すでに chef クライアントとして設定されているがホスト名が変わってしまう場合は、[chef-client-hostname].json ファイル名を割り当てられたホスト名に変更する。

適用したいクックブックが記述されているか、nodes/[chef-client-hostname].json ファイルの中身を確認。

## ノードに適用

ノードに適用。

<pre class="brush: plain; title: ; notranslate" title="">$ knife solo cook [chef-client-hostname]</pre>

AWS EC2 に適用する場合はキーとユーザ名の指定が必要なので、ec2-user ユーザで作業すると、

<pre class="brush: plain; title: ; notranslate" title="">$ knife solo cook -i ~/Keypair/[key-file] ec2-user@[chef-client-hostname]</pre>

とする。