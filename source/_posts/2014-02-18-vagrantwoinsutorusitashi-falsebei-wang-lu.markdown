---
layout: post
title: "vagrantをインストールした時の備忘録"
date: 2014-02-18 18:45:27 +0900
comments: true
categories: vagrant, development, VM
---

#Vagrant

参考文献
http://d.hatena.ne.jp/naoya/20130205/1360062070

## Vagrant をインストール

VirtualBoxをインストールしておく。(http://virtualbox.org)

さらに http://download.vagrantup.com から必要なインストーラーをダウンロードして、vagrantをインストールしてやる。

## Vagrant のセットアップ

### Vagrantの大枠となる box を用意する

なければ、サイトからも落とせる。各OS用に用意されている物でも良い。
>http://www.vagrantup.com/

そして、vagrant のおおもととなる物を準備してやる。サイトは Ubuntuをデフォルトに使っているっぽいが、うちは CentOSのほうがいいので、それを下記のサイトからとってくる(必要なの使ってね)。そして、次に `vagrant box` に追加してやる。
>http://www.vagrantbox.es/

```
vagrant box add #{name_of_box} https://github.com/2creatives/vagrant-centos/releases/download/v6.5.1/centos65-x86_64-20131205.box
```

### Vagrantの設定を用意する

次は `vagrant init ` で設定ファイルを作ってやる。基本的にはvagrant用のフォルダを用意して、そこで `vagrant init` コマンドを叩くと、 Vagrantfile が作成される。
そこから `vagrant up` を叩くと vagrantのインスタンスが構築され、仮想環境が立ち上がる。


### Vagrantfile について

Vagrantfile は vagrant がどのようなマシンが仮想的に作られるべきかを記すファイルになる。

ひとつのプロジェクトにつき、ひとつの Vagrantfile で動き、バージョンコントールされているべきである。

その時に注意すべき点は、.vagrant ファイルを管理しない事。そのため、下記の.gitignoreを追加しておく。

```:.gitignore
# .gitignore
--------------
# vagrant gitignore
.vagrant
```

* どうでもいい注意点だが、centos でデフォルトで iptablesがあがってて外部からアクセスしようとして失敗したりしてはまらないようにメモ。
